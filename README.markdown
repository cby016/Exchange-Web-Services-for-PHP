Exchange Web Services for PHP
=============================

This is a basic library that abstracts a few of common operations using the Exchange Web Services API present on Exchange Server 2007. It is based in large part on [Talking SOAP with Exchange](http://www.howtoforge.com/talking-soap-with-exchange).

Right now only basic operations are supported (getting a list of all email from a user's inbox (including attachments), sending email messages, getting calendar events, and utilizing Delegate support to access multiple accounts), but as time goes on hopefully more funtionality will be added.

Installation
------------

**Basic Installation**

You can give this a shot, but it doesn't work for me on my Exchange server. If it doesn't work for you, don't panic, just see the "Advanced Installation" section below.

The basic installation is basically just to tell the ExchangeClient to look on the Exchange Server itself for the SOAP files. You would do so during initialization, like so:

	$exchangeclient = new Exchangeclient();
	$exchangeclient->init("mailbox_username", "mailbox_password", NULL, "http://exchange.server.local/EWS/Services.wsdl");

If that works for you, great. If not, then you had the same problem as me, see the Advanced section below.

**Advanced Installation**

Installation is a bit more complicated than I'd like, but most of that is due to the way the API is set up.

The first thing you must do is download the SOAP files from your Exchange server. Those files are located here (be sure to replace "exchange.server.local" with the address of the Exchange server on your network):

http://exchange.server.local/EWS/Services.wsdl

http://exchange.server.local/EWS/messages.xsd

http://exchange.server.local/EWS/types.xsd

Download those three files, and place them in the same directory as the "Exchangeclient.php" file.

Finally, open up the Services.wsdl file in a TEXT EDITOR (do not use a Word Processor like MS Word), and at the very bottom of the file, before the final </wsdl:definitions> tag, add the following:

	<wsdl:service name="ExchangeServices">
		<wsdl:port name="ExchangeServicePort" binding="tns:ExchangeServiceBinding">
			<soap:address location="http://exchange.server.local/EWS/Exchange.asmx"/>
		</wsdl:port>
	</wsdl:service>

Be sure to replace the exchange.server.local with your actual exchange server address.

That's it! You should now be ready to use the library. Just include it in your PHP script, and you're off. 

Example
-------

The following code initializes the library and sends off a quick test messsage:

	$exchangeclient = new Exchangeclient();
	$exchangeclient->init("mailbox_username", "mailbox_password");
	$exchangeclient->send_message("you@otherdomain.com", "Subject", "A test message");


CodeIgniter
----------
This code is compatible with CodeIgntier. To use, just drop the Exchangeclient.php file (as well as the 3 SOAP definition files from the installation instructions) into your "application/libraries" folder. Then use:

	$this->load->library("Exchangeclient");
	$this->exchangeclient->init("mailbox_username", "mailbox_password");
	etc...


License
-------

This project is licensed under the MIT License. See the included LICENSE files for more details.
