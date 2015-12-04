---
layout: post
title: "Client Side Validations"
date: 2013-06-18 01:29
comments: true
categories: 
---

<p>The <a href='http://railscasts.com/episodes/263-client-side-validations'>Railscast</a> for the <a href='https://github.com/bcardarella/client_side_validations'>Client Side Validations Gem</a> is very outdated and does not function.  Furthermore, the documentation for the gem did not completely work with my version of Rails, 3.2.13.  The point of this post is to get front end validations to work with Rails 3.2.13.</p>

<code>gem 'client_side_validations'</code>

<code>$ rails g client_side_validations:install</code>


<p>The file below is created, but you must uncomment lines 10 through 16 in order to get it to work.</p>

{% codeblock config/initializers/client_side_validations.rb lang:ruby %}

# ClientSideValidations Initializer

# Uncomment to disable uniqueness validator, possible security issue
# ClientSideValidations::Config.disabled_validators = [:uniqueness]

# Uncomment to validate number format with current I18n locale
# ClientSideValidations::Config.number_format_with_locale = true

# Uncomment the following block if you want each input field to have the validation messages attached.
ActionView::Base.field_error_proc = Proc.new do |html_tag, instance|
  unless html_tag =~ /^<label/
    %{<div class="field_with_errors">#{html_tag}<label for="#{instance.send(:tag_id)}" class="message">#{instance.error_message.first}</label></div>}.html_safe
  else
    %{<div class="field_with_errors">#{html_tag}</div>}.html_safe
  end
end

{% endcodeblock %}

<p><code>//= require rails.validations</code><br />...in applications.js<br />...and the following to your form.</p>

{% codeblock _form.html.erb lang:ruby %}
<%= form_for @user, :validate => true do |f| %>
{% endcodeblock %}
<br />

<p>And a little CSS...</p><br />

{% codeblock application.css lang:css %}
.field_with_errors .message {
	color: #FFC200;
	padding-left: 10px;
	display: inline;
}
{% endcodeblock %}

<p>Your app should now be able to display Rails error messages in line with your form fields.  This is where I began to run into problems, creating a custom validator.</p><br />
<p>The Github Documentation says to put the following code into app/validators/email_validator.rb</p><br />

{% codeblock initializers/email_validator.rb lang:ruby %}
class EmailValidator < ActiveModel::EachValidator
  def validate_each(record, attr_name, value)
    unless value =~ /^([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})$/i
      record.errors.add(attr_name, :email, options.merge(:value => value))
    end
  end
end

# This allows us to assign the validator in the model
module ActiveModel::Validations::HelperMethods
  def validates_email(*attr_names)
    validates_with EmailValidator, _merge_attributes(attr_names)
  end
end
{% endcodeblock %}

<p>This did not work for me.  However, creating the email_validator.rb file under the initializers directory did the trick.  I need to ask a Flatiron School compadre why this is.  Follow the rest of the custom validator documentation for awesome validations.  I hope this saves people time while implenting this gem.</p>

