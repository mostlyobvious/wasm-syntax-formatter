# frozen_string_literal: true

file 'ruby.wasm' => %w[Gemfile.lock] do
  `bundle exec rbwasm build -o ruby.wasm --build-profile full --ruby-version 3.4`
end

task default: ['ruby.wasm']

task preview: ['ruby.wasm'] do
  `ruby -run -e httpd . -p 8000`
end
