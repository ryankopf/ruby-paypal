# Ruby-PayPal

## Set it up in your rails application

In your gemfile:

`gem "ruby-paypal", :git => "git://github.com/Reprazent/ruby-paypal.git"`


## To use Ruby-PayPal

It's critical that you understand how PayPal works and how the PayPal NVP API
works. You should be relatively well-versed in the NVP API Developer Guide and
Reference (https://www.paypal.com/en_US/ebook/PP_NVPAPI_DeveloperGuide/index.html).
You should also visit and register yourself with the PayPal Developer Network
and get a Sandbox account with in the PayPal Development Central
(https://developer.paypal.com/).

Note that this library only supports the API signature method of securing the API credentials.

Using the Ruby-PayPal library is relatively simple:

## Initialize a paypal object

```ruby
paypal = Paypal.new(<username>,<password>,<signature>,<url>)
```
The url is the paypal API to use: sandbox for development, normal for production:

```
        development:
          username: username
          password: pass
          signature: sig
          url: sandbox
          action: https://www.sandbox.paypal.com/cgi-bin/webscr
        test:
          username: username
          password: pass
          signature: sig
          url: sandbox
          action: https://www.sandbox.paypal.com/cgi-bin/webscr
        staging:
          username: username
          password: pass
          signature: sig
          url: sandbox
          action: https://www.sandbox.paypal.com/cgi-bin/webscr
        production:
          username: "username"
          password: "password"
          signature: "signature"
          url: production
          action: https://wwwpaypal.com/cgi-bin/webscr
```

### For the Direct Payment using credit card payment, you need to use the
DoDirectPayment APIs:

	username = <PayPal API username>
	password = <PayPal API password>
	signature = <PayPal API signature>

	ipaddress = '192.168.1.1' # can be any IP address
	amount = '100.00' # amount paid
	card_type = 'VISA' # can be Visa, Mastercard, Amex etc
	card_no = '4512345678901234' # credit card number
	exp_date = '022010' # expiry date of the credit card
	first_name = 'Sau Sheong'
	last_name = 'Chang'

    paypal = Paypal.new(username, password, signature)
    response = paypal.do_direct_payment_sale(ipaddress, amount, card_type,
			   card_no, exp_date, first_name, last_name)
	if response.ack == 'Success' then
	  # do your thing
	end

The above code is for a final sale only.

Note that the credit card number is checked against a modulo-10 algorithm (Luhn check) as well as a simple credit card
type check. For more information please refer to http://en.wikipedia.org/wiki/Luhn_algorithm and
http://en.wikipedia.org/wiki/Credit_card_number

## For the Express Checkout using the customer's PayPal account for payment, you will need to use the ExpressCheckout APIs (Updated!)

You can get a paypal token trough `paypal.do_set_express_checkout(return_url, cancel_url, products, currency="USD", other_params={})`

The products parameter is an array of hashes like this:

`{:price => 1.99, :name => "NAME", :quantity => 1, :description => "DESCRIPTION"}`

Using the token returned you can build URL's that will allow the user to pay trough paypal, get the payment info from paypal like this:

`paypal.do_get_express_checkout_details(token)`

Using the info returned you can do the actual payment:

`paypal.do_express_checkout_payment(token,'Sale', payer_id, amount)`

The amount parameter needs to be the same as the one calculated from the products passed when getting the token.

## Source

## Originally by:

- Author:  Chang Sau Sheong  (mailto:sausheong.chang@gmail.com)
- Author:  Philippe F. Monnet (mailto:pfmonnet@gmail.com)

The original source can be found on http://rubyforge.org/projects/ruby-paypal/


## License
The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## WARRANTY

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## LIABILITY

IN NO EVENT WILL ANY COPYRIGHT HOLDER, OR ANY OTHER PARTY WHO MODIFIES
AND/OR CONVEYS THE PROGRAM AS PERMITTED ABOVE, BE LIABLE TO YOU FOR
DAMAGES, INCLUDING ANY GENERAL, SPECIAL, INCIDENTAL OR CONSEQUENTIAL
DAMAGES ARISING OUT OF THE USE OR INABILITY TO USE THE PROGRAM (INCLUDING
BUT NOT LIMITED TO LOSS OF DATA OR DATA BEING RENDERED INACCURATE OR
LOSSES SUSTAINED BY YOU OR THIRD PARTIES OR A FAILURE OF THE PROGRAM
TO OPERATE WITH ANY OTHER PROGRAMS), EVEN IF SUCH HOLDER OR OTHER PARTY
HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGES.