+++
fragment = "contact"
slot = "content/after-content"
weight = 10

background = "light"
form_name = "contact-form"

title = "Leave a comment"

post_url = "https://example.com/mailout"
email = "mail@example.com"
button_text = "Submit"

[message]
  success = "Thank you for submitting your comment."
  error = "Message could not be send. Please contact us at mail@example.com instead."

[fields.name]
  text = "Your Name *"
  error = "Please enter your name"

[fields.email]
  text = "Your Email *"
  error = "Please enter your email address"

[fields.message]
  text = "Your Message *"
  error = "Please enter a message"

[[fields.hidden]]
  name = "page"
+++
