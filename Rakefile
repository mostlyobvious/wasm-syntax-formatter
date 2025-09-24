# frozen_string_literal: true

release = 'ruby-3.4-wasm32-unknown-wasip1-minimal'

file "build/#{release}" do
  filename = "#{release}.tar.gz"
  `curl -LO https://github.com/ruby/ruby.wasm/releases/latest/download/#{filename}`
  `tar xzf #{filename} -C build`
  rm filename
end

file 'ruby.wasm' => ["build/#{release}"] do
  cp "build/#{release}/usr/local/bin/ruby", 'ruby.wasm'
end

file 'app.wasm' => %w[Gemfile.lock ruby.wasm src/format.rb] do
  require 'bundler/setup'

  cp_r $:.find { _1.include?('syntax_tree') }, '.'
  cp_r $:.find { _1.include?('prettier_print') }, '.'
  cp_r $:.find { _1.include?('rouge') }, '.'

  `rbwasm pack ruby.wasm --dir ./src::/src --dir ./lib::/lib --dir ./build/#{release}/usr::/usr -o app.wasm`
  rm_rf 'lib'
end

task default: ['app.wasm']

task preview: ['app.wasm'] do
  puts `wasmtime app.wasm /src/format.rb`
end
