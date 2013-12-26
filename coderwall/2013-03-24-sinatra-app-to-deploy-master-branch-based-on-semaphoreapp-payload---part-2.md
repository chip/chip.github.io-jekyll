# Sinatra app to deploy master branch based on SemaphoreApp payload - Part 2

Since my previous post on this topic, [Sinatra app to deploy master branch
based on SemaphoreApp
payload](http://blog.chipcastle.com/2013/02/13/sinatra-app-to-deploy-master-branch-based-on-semaphoreapp-payload/),
I've had a chance to see this app in action and discovered a couple of
glitches, so I wanted to address them here.  Plus, I thought it would be a
great time to do a little refactoring to make the code more readable and
maintainable.

First, the output wasn't being logged in a consistent manner, so I decided to
switch over to the **open3** Ruby library.

Next, sometimes the temporary working directory that contained the git
repository would get into a dirty state, so I've addressed that by resetting the branch before pulling in the `command` method below.

Here's the full source for the **app.rb** script:


### app.rb

    require 'open3'

    WORKSPACE = '/var/www/deploy/whatevs_repo_clone'

    post '/deploy' do
      begin
        write_log "Received post: branch #{data["branch_name"]} #{data["result"]}"

        if master_branch_passed?
          message_campfire deploy_notification, room
          deploy
        else
          write_log "SKIPPING DEPLOYMENT FOR #{data['branch_name']} with a result of #{data['result']}"
        end
      rescue => e
        write_log e.inspect
      end
    end

    get '/' do
      "Whatchu talkin bout, Willis?"
    end

    def write_log(log_message, filename = :master)
      File.open("log/deploy_#{filename.to_s}.log", "a") do |file|
        file.write "[#{current_time}] #{log_message.to_s}\n"
      end
    end

    def current_time
      Time.now.strftime("%Y-%m-%d %h:%m:%s")
    end

    def message_campfire(message, room)
      write_log "Messaged Campfire: #{message}"
      room.speak message
    end

    def campfire
      @campfire ||= Tinder::Campfire.new account, :token => token, :ssl_verify => false
    end

    def account
      ENV['CAMPFIRE_ACCOUNT']
    end

    def token
      ENV['CAMPFIRE_TOKEN']
    end

    def room
      @room ||= campfire.find_room_by_name("Whatevs")
    end

    def json
      request.body.read
    end

    def data
      @data ||= Yajl::Parser.parse(json)
    end

    def deploy_notification
      "Deploying #{data['branch_name']} to staging"
    end

    def master_branch_passed?
      data['branch_name'] == 'master' && data['result'] == 'passed'
    end

    def command
      @command ||= [
        "cd #{WORKSPACE}",
        "git reset --hard origin",
        "git pull origin master",
        "bundle install",
        "bundle exec cap staging deploy:migrations",
      ].join(" && ")
    end

    def deploy
      Bundler.with_sparkling_clean_env do
        write_log "Executing: #{command}"
        executed = Open3.capture3(command)
        write_log "stdout: #{executed[0]}"
        write_log "stderr: #{executed[1]}"
        write_log "success?: #{executed[2].success?}"
      end
      room.speak "Deployment finished: #{command}"
      write_log "DONE"
    end

    module Bundler
      class << self
        def with_sparkling_clean_env
          oenv = ENV.to_hash

          %w{BUNDLE_GEMFILE RUBYOPT GEM_HOME GIT_DIR GIT_WORK_TREE}.each { |key| ENV.delete(key) }

          yield

          ENV.replace(oenv)
        end
      end
    end
    
I like that a lot better since it's much easier to follow and improve.

**If you want to see how to configure this script properly on your staging
server**, please see part 1 of this post for the original implementation that
includes the Gemfile, config.ru and Apache vhosts configuration: [Sinatra app
to deploy master branch based on SemaphoreApp
payload](http://blog.chipcastle.com/2013/02/13/sinatra-app-to-deploy-master-branch-based-on-semaphoreapp-payload/).


