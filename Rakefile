# Copyright 2009 Sidu Ponnappa

# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
# Unless required by applicable law or agreed to in writing, software distributed under the License
# is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and limitations under the License.

gem 'rspec'
require 'rake'
require 'rake/rdoctask'
require 'spec'
require 'spec/rake/spectask'
require 'rcov'
require 'rcov/rcovtask'

desc 'Default: run spec tests.'
task :default => :spec

desc "Run all specs"
Spec::Rake::SpecTask.new(:spec) do |task|
  task.spec_files = FileList['spec/wrest/**/*_spec.rb']
  task.spec_opts = ['--options', 'spec/spec.opts']
end

desc 'Generate documentation for Wrest'
Rake::RDocTask.new(:rdoc) do |rdoc|
  rdoc.rdoc_dir = 'doc'
  rdoc.title = 'WRest'
  rdoc.options << '--line-numbers' << '--inline-source'
  rdoc.rdoc_files.include('README.rdoc')
  rdoc.rdoc_files.include('lib/**/*.rb')
end

desc "Run all specs in spec directory with RCov"
Spec::Rake::SpecTask.new(:rcov) do |t|
  t.spec_opts = ['--options', "spec/spec.opts"]
  t.spec_files = FileList["spec/wrest/**/*_spec.rb"]
  t.rcov = true
  t.rcov_opts = lambda do
    IO.readlines("spec/rcov.opts").map {|l| l.chomp.split " "}.flatten
  end
  # t.verbose = true
end

begin
  require 'jeweler'
  Jeweler::Tasks.new do |gemspec|
    gemspec.name = "wrest"
    gemspec.version = "0.0.3"
    gemspec.summary = "REST client library for Ruby."
    gemspec.description = "Wrest is a REST client library which allows you to quickly build object oriented wrappers around any web service. It has two main components - Wrest Core and Wrest::Resource."
    gemspec.authors = ["Sidu Ponnappa"]
    gemspec.email = "ckponnappa@gmail.com"
    gemspec.homepage = "http://github.com/kaiwren/wrest"
    gemspec.has_rdoc = true
    gemspec.platform = Gem::Platform::RUBY
    gemspec.executables = ['wrest']
    gemspec.require_path = "lib"
    gemspec.files.exclude 'spec/wrest/meh_spec.rb'
    gemspec.test_files.exclude 'spec/wrest/meh_spec.rb'
    gemspec.add_dependency('activesupport', '>= 2.1.0')    
    gemspec.add_dependency('json', '>= 1.1.3')    
    gemspec.add_dependency('xml-simple', '>= 1.0.11')    
  end
rescue LoadError
  puts "Jeweler not available. Install it with: sudo gem install technicalpickles-jeweler -s http://gems.github.com"
end

namespace (:benchmark) do
  desc "Create classes to be used in Wrest::Resource vs. ActiveResource"
  task :setup_test_classes do
    require 'active_resource'
    require 'wrest'
    
    class Ooga < Wrest::Mappers::Resource::Base;end
    class Booga < ActiveResource::Base; self.site='';end
  end
  
  desc "Benchmark when objects are created each time before getting data; i.e there are few queries per instantiation"
  task :create_and_get => :setup_test_classes do |t|    
    
    n = 10000
    puts "Running #{n} times per report"
    Benchmark.bmbm(10) do |rpt|
      rpt.report("Wrest::Resource") do
        n.times { 
          ooga = Ooga.new(:id => 5, :profession => 'Natural Magician', :enhanced_by => 'Kai Wren')
          ooga.profession
          ooga.profession?
          ooga.enhanced_by 
          ooga.enhanced_by? 
        }
      end

      rpt.report("ActiveResource") do
        n.times { 
          booga = Booga.new(:id => 5, :profession => 'Natural Magician', :enhanced_by => 'Kai Wren')
          booga.profession
          booga.profession?
          booga.enhanced_by 
          booga.enhanced_by?
        }
      end
    end
  end
  
  desc "Benchmark when objects are created beforehand; i.e there are many queries per instantiation"
  task :create_once_and_get => :setup_test_classes do |t|    

    n = 10000
    puts "Running #{n} times per report"
    
    Benchmark.bmbm(10) do |rpt|      
      rpt.report("Wrest::Resource") do
        ooga = Ooga.new(:id => 5, :profession => 'Natural Magician', :enhanced_by => 'Kai Wren')
        
        n.times { 
          ooga.profession
          ooga.profession?
          ooga.enhanced_by 
          ooga.enhanced_by?
        }
      end

      rpt.report("ActiveResource") do
        booga = Booga.new(:id => 5, :profession => 'Natural Magician', :enhanced_by => 'Kai Wren')
                
        n.times { 
          booga.profession
          booga.profession?
          booga.enhanced_by 
          booga.enhanced_by?
        }
      end
    end
  end
  
  desc "Benchmark objects respond_to? performance without invocation"
  task :responds_to_before => :setup_test_classes do |t|    

    n = 10000
    puts "Running #{n} times per report"
    
    Benchmark.bmbm(10) do |rpt|      
      rpt.report("Wrest::Resource") do
        ooga = Ooga.new(:id => 5, :profession => 'Natural Magician', :enhanced_by => 'Kai Wren')
        
        n.times { 
          ooga.respond_to?(:profession)
          ooga.respond_to?(:profession?)
          ooga.respond_to?(:profession=)
        }
      end

      rpt.report("ActiveResource") do
        booga = Booga.new(:id => 5, :profession => 'Natural Magician', :enhanced_by => 'Kai Wren')
                
        n.times { 
          booga.respond_to?(:profession)
          booga.respond_to?(:profession?)
          booga.respond_to?(:profession=)
        }
      end
    end
  end

  desc "Benchmark objects respond_to? performance after invocation"
  task :responds_to_after => :setup_test_classes do |t|    

    n = 10000
    puts "Running #{n} times per report"
    
    Benchmark.bmbm(10) do |rpt|      
      rpt.report("Wrest::Resource") do
        ooga = Ooga.new(:id => 5, :profession => 'Natural Magician', :enhanced_by => 'Kai Wren')
        ooga.profession
        ooga.profession?
        ooga.profession = ''
        
        n.times { 
          ooga.respond_to?(:profession)
          ooga.respond_to?(:profession?)
          ooga.respond_to?(:profession=)
        }
      end

      rpt.report("ActiveResource") do
        booga = Booga.new(:id => 5, :profession => 'Natural Magician', :enhanced_by => 'Kai Wren')
        booga.profession
        booga.profession?
        booga.profession = ''
                        
        n.times { 
          booga.respond_to?(:profession)
          booga.respond_to?(:profession?)
          booga.respond_to?(:profession=)
        }
      end
    end
  end
end