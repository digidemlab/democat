# Rakefile

require 'yaml'
require 'digest/md5'
require 'RMagick'

namespace :assets do
  desc "Generate favicons using your Gravatar"
  task :favicons do
    gravatar_email = 'petter@p-jo.se'
    asset_path = 'public'
    name_pre = "apple-touch-icon-%dx%d-precomposed.png"

    origin = "origin.png"

    [144, 114, 72, 57, 32].each do |size|
      im = Magick::Image::read(origin).first.resize(size, size)
      circle = Magick::Image.new size, size
      gc = Magick::Draw.new
      gc.fill 'black'
      gc.circle size/2, size/2, size/2, 1
      gc.draw circle
      mask = circle.blur_image(0,1).negate
      mask.matte = false
      im.matte = true
      im.composite!(mask, Magick::CenterGravity, Magick::CopyOpacityCompositeOp)
      if size > 32
        puts "Generating #{name_pre} icon..." % [size, size]
        im.write "#{asset_path}/" + name_pre % [size, size]
      else
        puts "Generating favicon.ico..."
        im.write "#{asset_path}/favicon.ico"
      end
    end

    puts "Cleaning up..."
    File.delete origin
  end
end
