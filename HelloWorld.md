LGS request: Configure HTTPD to disable TRACE method.

Description: The HTTP TRACE method is normally used to return the full HTTP request back to the requesting client for proxy-debugging purposes. An attacker can create a webpage using XMLHTTP, ActiveX, or XMLDOM to cause a client to issue a TRACE request and capture the client's cookies. This effectively results in a Cross-Site Scripting attack.

Verification: Create a test file called trace.txt with the following content. There must be one blank line at the end of the file. Use the actual IP address in the Host: line.

TRACE /web/content/login.html/ HTTP/1.1
Host: actual_omniswitch_ip_address
Connection: Close
Cookie: Vulnerable=yes

Execute the following commands using the actual IP address and file containing the previous content.

ncat actual_omniswitch_ip_address 80 < trace.txt
ncat –-ssl actual_omniswitch_ip_address 443 <trace.txt

If either command response contains “Cookie: Vulnerable=yes”, then target has the vulnerability and it can be exploited.