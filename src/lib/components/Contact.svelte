<form
	class="flow"
	name="contact-form"
	action="/contact/success"
	method="POST"
	netlify-honeypot="bot-field"
	data-netlify="true"
>
	<input type="hidden" name="form-name" value="contact-form" />

	<div class="form-section">
		<label for="name">Name</label>
		<input name="name" id="name" required placeholder="Your Name" type="text" />
	</div>
	<div class="form-section">
		<label for="email">Email</label>
		<input name="email" id="email" required placeholder="Your Email" type="email" />
	</div>
	<div class="form-section">
		<label for="message">Message</label>
		<textarea name="message" id="message" required placeholder="Your Message" rows="8" cols="65" />
	</div>

	<input type="submit" value="Submit" class="align-right" />
</form>
