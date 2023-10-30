#
# SSL Certificate generation for artifactory server.
#

# Generate Private Key.
openssl genrsa -out artifactory.mydomain.com.key 4096

# Generate CSR.
openssl req -new -key artifactory.mydomain.com.key -out artifactory.mydomain.com.csr

# Now use this CSR file to buy the SSL certificates from CA.

# Or create self-signed.
openssl x509 -in  artifactory.mydomain.com.csr -out  artifactory.mydomain.com.crt -req -signkey  artifactory.mydomain.com.key -days 365

# Once done copy these certifcates to certs directory ( configs/certs ) before running container.

