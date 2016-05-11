NAME='vertx'
DESCRIPTION='Vert.x is a tool-kit for building reactive applications on the JVM.'
VERSION='3.2.1'
BUILD_DIR='./build'
OUT_DIR='./pkg'
PREFIX='/opt'

# Setup RUBY ENV
ENV['PATH'] =  "#{Dir.pwd}/.gems/bin:#{ENV['PATH']}"
ENV['GEM_HOME'] = "#{Dir.pwd}/.gems"
ENV['GEM_PATH'] = '/var/lib/gems/2.3.0'

desc 'Install Dependencies'
task :install_deps do
  Dir.mkdir "#{Dir.pwd}/.gems" unless File.directory? "#{Dir.pwd}/.gems"
  sh %{ bundle update }
end

desc 'Clean'
task :clean do
  sh %{ rm -rf *.tar.gz }
  sh %{ rm -rf ./build }
  sh %{ rm -rf ./pkg }
end

desc 'Fetch Binary'
task :fetch do
  sh %{ wget https://bintray.com/artifact/download/vertx/downloads/vert.x-#{VERSION}-full.tar.gz }
end

desc 'Build'
task :build do
  sh %{ mkdir -p #{BUILD_DIR}#{PREFIX} }
  sh %{ mkdir -p #{BUILD_DIR}/usr/bin }
  sh %{ ln -s #{PREFIX}/#{NAME}/bin/#{NAME} #{BUILD_DIR}/usr/bin/#{NAME} }
  sh %{ tar -xf vert.x-#{VERSION}-full.tar.gz }
  sh %{ mv #{NAME} #{BUILD_DIR}#{PREFIX} }
end

desc 'Package'
task :pkg do
  sh %{ mkdir ./pkg }
  sh %{
    fpm -t deb \
              -s dir \
              -n #{NAME} \
              -v #{VERSION} \
              --description "#{DESCRIPTION}" \
              --deb-no-default-config-files \
              -a all \
              -d java-runtime-headless \
              --deb-user root \
              --deb-group root \
              -p #{OUT_DIR} \
              -C #{BUILD_DIR} \
              .
  }
end

task :default => [:install_deps, :clean, :fetch, :build, :pkg]

