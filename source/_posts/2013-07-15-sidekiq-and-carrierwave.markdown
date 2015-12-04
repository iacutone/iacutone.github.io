---
layout: post
title: "Sidekiq and Carrierwave, Part 3"
date: 2013-07-15 20:44
comments: true
categories: Rails Sidekiq
---

<p>I decided to use the <a href='https://github.com/lardawge/carrierwave_backgrounder'>Carrierwave Backgrounder Gem</a> in order to asynchronously upload photos to Amazon S3.  This is a different gem than the one Ryan Bates uses in his Railscast.  I was having difficulty implimenting the <a href='https://github.com/dwilkie/carrierwave_direct'>Carrierwave Direct Gem</a>  However, the Backgrounder Gem works great! This is the last installment of the three part series on uploading photos.</p>

<p>This is the final version of the avatar_uploader.rb file.  Just add line 3, which is in the Backgrounder documentaion.</p>

{% codeblock avatar_uploader.rb lang: ruby %}

  class AvatarUploader < CarrierWave::Uploader::Base
  # include CarrierWaveDirect::Uploader
  include ::CarrierWave::Backgrounder::Delay

  # Include RMagick or MiniMagick support:
  include CarrierWave::RMagick
  # include CarrierWave::MiniMagick

  # Include the Sprockets helpers for Rails 3.1+ asset pipeline compatibility:
  include Sprockets::Helpers::RailsHelper
  include Sprockets::Helpers::IsolatedHelper

  # Choose what kind of storage to use for this uploader:
  # storage :file
  storage :fog

  include CarrierWave::MimeTypes
  process :set_content_type

  # Override the directory where uploaded files will be stored.
  # This is a sensible default for uploaders that are meant to be mounted:
  # def store_dir
  #   "uploads/#{model.class.to_s.underscore}/#{mounted_as}/#{model.id}"
  # end

  # Provide a default URL as a default if there hasn't been a file uploaded:
  # def default_url
  #   # For Rails 3.1+ asset pipeline compatibility:
  #   # asset_path("fallback/" + [version_name, "default.png"].compact.join('_'))
  #
  #   "/images/fallback/" + [version_name, "default.png"].compact.join('_')
  # end

  # Process files as they are uploaded:
  # process :scale => [200, 300]
  #
  # def scale(width, height)
  #   # do something
  # end

  # Create different versions of your uploaded files:
  version :thumb do
    process :resize_to_limit => [150, 150]
  end

  version :profile do
    process :resize_to_limit => [200, 200]
  end

  # Add a white list of extensions which are allowed to be uploaded.
  # For images you might use something like this:
  # def extension_white_list
  #   %w(jpg jpeg gif png)
  # end

  # Override the filename of the uploaded files:
  # Avoid using model.id or version_name here, see uploader/store.rb for details.
  # def filename
  #   "something.jpg" if original_filename
  # end
  end

{% endcodeblock %}

<p>Also, add this code to your model.<p>

{% codeblock profile.rb lang:ruby %}

	mount_uploader :avatar, AvatarUploader
  process_in_background :avatar

{% endcodeblock %}

<p>Following the install instructions, this file is created in the initializers directory.<p>

{% codeblock initializers/carrierwave_backgrounder.rb lang:ruby %}

	CarrierWave::Backgrounder.configure do |c|
	  # c.backend :delayed_job, queue: :carrierwave
	  # c.backend :resque, queue: :carrierwave
	  c.backend :sidekiq, queue: :carrierwave
	  # c.backend :girl_friday, queue: :carrierwave
	  # c.backend :qu, queue: :carrierwave
	  # c.backend :qc
	end

{% endcodeblock %}

<p>Then boot up Sidekiq to listen for jobs with sidekiq -q carrierwave command in your terminal.  You might need to start your Redis server with the command redis-server.  Your background worker should now asynchronously process background jobs!</p>

<p>Sidekiq also has a cool interface to show what workers are up, how many successes and failures there have been and some other neat features.  It is real easy to set up.  First add the following code to your Gemfile.</p>

{% codeblock Gemfile lang:ruby %}

	gem 'slim', '>= 1.1.0'
	gem 'sinatra', '>= 1.3.0', :require => nil

{% endcodeblock %}

<p>Then create this route with the require line before the beginning of the block.</p>

{% codeblock routes.rb lang:ruby %}

	require 'sidekiq/web'

 	mount Sidekiq::Web => '/sidekiq'
  
{% endcodeblock %}

<p>I really like this gem and recommend it to anyone trying to run background processes to upload pictures via Carrierwave.</p>

<p>h/t Blake Johnson</p>
