require "spec/rake/spectask"
require "rake/clean"
require "rake/rdoctask"
require "benchmark"
require "rake/gempackagetask"

desc "Run all specs"
Spec::Rake::SpecTask.new do |t|
  t.spec_files = FileList["spec/**/*_spec.rb"]
  t.spec_opts = ["-o", "spec/spec.opts"]
  t.rcov = true
  t.rcov_dir = 'doc/coverage'
  t.rcov_opts = ['--exclude', 'spec\/spec,spec\/.*_spec.rb', "-T"]
end

CLOBBER.include(
  "doc/coverage"
)

namespace :spec do
  desc "Run all specs and store html output in doc/specs.html"
  Spec::Rake::SpecTask.new('html') do |t|
    t.spec_files = FileList['spec/**/*_spec.rb']
    t.spec_opts = ['--diff','--format html','--backtrace','--out doc/specs.html']
  end
end

desc 'Generate RDoc'
rd = Rake::RDocTask.new do |rdoc|
  rdoc.rdoc_dir = 'doc/rdoc'
  rdoc.options << '--title' << 'faster-builder' << '--line-numbers' << '--inline-source' << '--main' << 'README'
  rdoc.template = ENV['TEMPLATE'] if ENV['TEMPLATE']
  rdoc.rdoc_files.include('README', 'MIT-LICENSE', 'CHANGELOG', 'lib/**/*.rb')
end

PKG_NAME = "faster-builder"
PKG_VERSION   = "1.0.0"
PKG_FILE_NAME = "#{PKG_NAME}-#{PKG_VERSION}"
PKG_FILES = FileList[
  '[A-Z]*',
  'lib/**/*.rb', 
  'spec/**/*.rb'
]

spec = Gem::Specification.new do |s|
  s.name = PKG_NAME
  s.version = PKG_VERSION
  s.summary = "A drop-in replacement for Builder::XmlMarkup which uses libxml for speed and security."
  s.description = <<-EOF
    Provides a Builder::XmlMarkup work-alike for libxml-ruby, giving you the
    ease of use of Builder with the speed and security of libxml.
  EOF
  
  s.add_dependency('libxml-ruby', '>= 0.5.4')
  
  s.files = PKG_FILES.to_a
  s.require_path = 'lib'

  s.has_rdoc = true
  s.rdoc_options = rd.options
  s.extra_rdoc_files = rd.rdoc_files.to_a
  
  s.authors = ["Coda Hale"]
  s.email = "coda.hale@gmail.com"
  s.homepage = "http://www.github.com/codahale/faster-builder"
  s.rubyforge_project = "faster-builder"
end

Rake::GemPackageTask.new(spec) do |pkg|
  pkg.need_zip = true
  pkg.need_tar = true
end