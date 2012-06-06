TastyAuth
=========

TastyAuth is an extremely tasty python module to integrate third-party
authentication schemes (read: Google, Twitter and Facebook auth).

TastyAuth is designed to be framework agnostic: it just exposes plain
functions that you can use *everywhere* WSGI is supported.


Show me teh code
----------------
A typical setup is the following:


    from bottle import route, redirect, request
    from tastyauth import Facebook, UserDenied, NegotiationError
    from pprint import pformat

    facebook = Facebook('fb-key', 'fb-secret',
                        'http://127.0.0.1:8080/facebook/callback', 'email')

    @route('/facebook/login')
    def facebook_login():
        # request.environ is a WSGI environment
        url = facebook.redirect(request.environ)
        redirect(url)

    @route('/facebook/callback')
    def facebook_callback(provider):
        try:
            # get the current user!
            user = facebook.get_user(request.environ)
        except UserDenied:
            # the user refused to log
            return 'User denied'
        except NegotiationError:
            # oops, something nasty happend
            return 'Negotiation error, maybe expired stuff'

        return '<pre>{0}</pre>'.format(pformat(user))


You can find a working example under the `example` dir.


Quick start
-----------

Clone the repo, `git clone git@github.com:twitter/bootstrap.git`,
or [download the latest release](https://github.com/twitter/bootstrap/zipball/master).

If you are using [virtualenv](http://www.virtualenv.org/) cd to the directory
containing `setup.py` and install TastyAuth's dependencies with:

    pip install -e .

Pip will install:

 * [WebOb](http://www.webob.org/)
 * [Bottle](https://github.com/defnull/bottle)


Request a key for authenticating with:
 * Facebok on https://developers.facebook.com/docs/reference/api/permissions/
 * Twitter on http://www.example.org/twitter/callback
 * Google on https://www.google.com/accounts/ManageDomains

Put all your Facebook, Twitter and Google keys somewhere in your configuration.

For every third-party service you wish to use for your webapp, you need to
define a redirect and a callback controller. The API is really easy to
use, please take a look to the example provided.

