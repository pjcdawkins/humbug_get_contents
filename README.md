file_get_contents
=================

Defines a `humbug_get_contents()` function that will transparently call `file_get_contents()`
except for HTTPS URIs where it will inject a context configured to enable secure
SSL/TLS requests on all versions of PHP less than 5.6. The function argument lists
are identical.

All versions of PHP below 5.6 disable SSL/TLS protections by default. This has led to
the spread of insecure uses of `file_get_contents()` to retrieve HTTPS resources. For example,
PHAR files or API requests. Without SSL/TLS protections, all such requests are vulnerable
to Man-In-The-Middle attacks.

This solution was originally implemented within the Composer Installer, so this is a
straightforward extraction of that code into a standalone package with just the one function.
It borrows functions from both Composer and Sslurp.

In rare cases, this function will complain when attempting to retrieve HTTPS URIs. This is
actually the point ;). An error should have two causes:

* A valid cafile could not be located, i.e. your server is misconfigured or missing a package
* The URI requested could not be verified, i.e. in a browser this would be a red page warning.

Neither is, in any way, a justification for disabling SSL/TLS and leaving end users vulnerable
to getting hacked. Resolve such errors; don't ignore or workaround them.