Certificate generation instructions
Create a CSR and sign it:

On client <foo>, create a key & CSR:
# openssl genrsa -out <foo>.key 2048

# cd /etc/pki/CA/newcerts
Edit /etc/pki/CA/newcerts/csr_template.txt; add the appropriate hostnames to the CN field and under the [ alt_names ] section (this is important; modern browsers require subjectAltNames to be populated via X509v3 extensions):

[ req ]
default_bits = 2048
prompt = no
default_md = sha256
req_extensions = v3_req
distinguished_name = dn

[ dn ]
C=US
ST=North Carolina
L=Raleigh
O=Home
OU=Backyard Goat Farm
emailAddress=mitch.foxworth@gmail.com
CN = bigip.dom.local

[ v3_req ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = bigip.dom.local
DNS.2 = bigip
IP.1 = 172.16.3.3

Create the CSR. Again, you need to request x509v3 extensions:
# openssl req -new -out <foo>.csr -key <foo>.key -config csr_template.txt

Verify the CSR contents, including the presence of the subjectAltName:
# openssl req -text -noout -in <foo>.csr

Copy the file to the CA (assumes CA machine is a Red Hat variant) machine and sign it:
# openssl x509 -req -in <foo>.csr -CA ../private/rootCA.pem -CAkey ../private/rootCA.key -CAcreateserial -out <foo>.crt -extensions v3_req -extfile csr_template.txt -days 3653 -sha256

To create a chained cert do:
# cat ../private/rootCA.pem >> <foo>.crt
