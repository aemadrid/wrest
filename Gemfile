source "http://rubygems.org"

gem 'hanna'
gem 'rubyforge'
gem 'simplecov', :platforms => :mri_19

group :multipart_support do
  gem 'multipart-post', '~> 1'
end

group :libcurl_support do
  # 1.8.7, 1.9.2, rbx
  platforms :ruby do
    gem 'patron', '~> 0.4'
  end
end

group :nokogiri do
  gem 'nokogiri', '~> 1'
end

group :libxml do
  platforms :ruby do
    gem 'libxml-ruby', '~> 1' unless Object.const_defined?('RUBY_ENGINE') && RUBY_ENGINE =~ /rbx/
  end
end
 
group :jrexml do
  platforms :jruby do
    gem 'jrexml', '~> 0.5.3'
  end
end

group :memcached_support do
  gem 'dalli', '~> 1.0.1'
end

#group :eventmachine_support do
#  gem 'eventmachine', '~> 0.12.10'
#end

gemspec
