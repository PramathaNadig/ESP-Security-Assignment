# ESP-Security-Assignment
Team name: TechWizards
Team GitHub id: https://github.com/PramathaNadig/ESP-Security-Assignment/tree/master
Team Members:
Aishwarya Lodhi - SJSU ID- 015960412	   email-aishwarya.lodhi@sjsu.edu
Blessy Dickson Daniel Moses	- SJSU ID- 016697460 email- blessy.dicksondanielmoses@ sjsu.edu
Pramatha Nadig Hassan Ravishankar		- SJSU ID- 016708588	email - pramathanadig.hassanravishankar@sjsu.edu
Rishikesh keshav Andhare	- SJSU ID- 	016726203	email - rishikeshkeshav.andhare@sjsu.edu
Srinishaa Prabhakaran 	- SJSU ID- 	016176914	email - srinishaa.prabhakaran@sjsu.edu

Abstract: We built a PKI infrastructure with an org that monitors the simple.org domain. It involves the Root CA, Signing CA and the TLS cert.
Steps : The PKI files are required to be cloned and then the directory should be changed.
1)	git clone https://bitbucket.org/stefanholek/pki-example-1
2)	cd pki-example-1
![image](https://user-images.githubusercontent.com/111613205/201600994-2d313dd7-fc5c-4bfd-a5c5-d081939b4385.png)

 
Root CA Creation:
To create the CA  resources, crl and certs follow the steps below:
1)	mkdir -p ca/root-ca/private ca/root-ca/db crl certs
2)	chmod 700 ca/root-ca/private

Database Creation: involves the following steps

1)	cp /dev/null ca/root-ca/db/root-ca.db
2)	cp /dev/null ca/root-ca/db/root-ca.db.attr
3)	echo 01 > ca/root-ca/db/root-ca.crt.srl
4)	echo 01 > ca/root-ca/db/root-ca.crl.srl
![image](https://user-images.githubusercontent.com/111613205/201601093-fd30c542-3b2a-407c-be76-ffbcef8b74b7.png)

 


Root CA Request: 
For creating the private key and the certificate signing request for the Root CA follow this command and the following commands for the configuration.
1)	openssl req -new \
2)	    -config etc/root-ca.conf \
3)	    -out ca/root-ca.csr \
4)	    -keyout ca/root-ca/private/root-ca.key
 ![image](https://user-images.githubusercontent.com/111613205/201601128-d54de7b5-1a69-4b13-8c2f-3d63b93fbc04.png)


Root CA Certificate
We should now utilize “openssl ca” command for the issuing of the self-signed root CA stationed with the certificate signing request based on the previous steps followed. Use the below commands.
1)	openssl ca -selfsign \
2)	    -config etc/root-ca.conf \
3)	    -in ca/root-ca.csr \
4)	    -out ca/root-ca.crt \
5)	    -extensions root_ca_ext
![image](https://user-images.githubusercontent.com/111613205/201601173-4eb6db7b-2694-42a4-96e0-cf8b4fda998c.png)
![image](https://user-images.githubusercontent.com/111613205/201601220-bbd135f1-c39a-4e09-88b7-0c6d5e40d3ec.png)
![image](https://user-images.githubusercontent.com/111613205/201601238-e887ed5b-37ec-49a3-85fe-741a0d34801c.png)

 
 

 



Signing CA Creation:
Steps for creating a new directory for the signing CA
1)	mkdir -p ca/signing-ca/private ca/signing-ca/db
2)	chmod 700 ca/signing-ca/private

Database Creation for Signing CA
Steps to be followed for the creation of the DB of the Signing CA
1)	cp /dev/null ca/signing-ca/db/signing-ca.db
2)	cp /dev/null ca/signing-ca/db/signing-ca.db.attr
3)	echo 01 > ca/signing-ca/db/signing-ca.crt.srl
4)	echo 01 > ca/signing-ca/db/signing-ca.crl.srl

 ![image](https://user-images.githubusercontent.com/111613205/201601280-b5fda6d8-d56d-4cf3-992b-e499826d73b7.png)








Signing CA Request
“openssl req -new” is the command that should be utilized for creating a private key and the signing request for the signing CA
Follow the below commands:
1)	openssl req -new \
2)	    -config etc/signing-ca.conf \
3)	    -out ca/signing-ca.csr \
4)	    -keyout ca/signing-ca/private/signing-ca.key
![image](https://user-images.githubusercontent.com/111613205/201601375-f8e3e430-2975-4535-bb74-5b55963c1060.png)
![image](https://user-images.githubusercontent.com/111613205/201601396-68d3bcbe-f83b-4346-a410-8b5b97ba29c6.png)
![image](https://user-images.githubusercontent.com/111613205/201601416-a4f84708-9d2b-46f7-abe3-3477ac16fcbb.png)

 
 
 
Signing CA Certificate
Follow the below commands for the creation of the Signing CA certificate
1)	openssl ca \
2)	    -config etc/root-ca.conf \
3)	    -in ca/signing-ca.csr \
4)	    -out ca/signing-ca.crt \
5)	    -extensions signing_ca_ext

 ![image](https://user-images.githubusercontent.com/111613205/201601479-57a9ffd0-75d1-4e0d-8b5c-82df7a52022c.png)
![image](https://user-images.githubusercontent.com/111613205/201601504-52f5ede5-24b0-441e-8c1c-696f2783cf6a.png)
![image](https://user-images.githubusercontent.com/111613205/201601523-caf0fd35-4961-411f-99a1-9c9b88540461.png)


 

 

Creation of TLS Server certificate
We have to create server request from TLS utilizing the below shown commands. For this we will create the private key and a signing certificate request for the certificate of the server.
Steps for creating the TLS Server certificate:
1)	SAN=DNS:www.simple.org \
2)	openssl req -new \
3)	    -config etc/server.conf \
4)	    -out certs/simple.org.csr \
5)	    -keyout certs/simple.org.key

 ![image](https://user-images.githubusercontent.com/111613205/201601571-210a32ce-956f-4861-a788-c6788a6a710c.png)



Now the certificate of the server would be issued with the below commands by utilizing the signing CA and csr from the previous step.
1)	openssl ca \
2)	    -config etc/signing-ca.conf \
3)	    -in certs/simple.org.csr \
4)	    -out certs/simple.org.crt \
5)	    -extensions server_ext
![image](https://user-images.githubusercontent.com/111613205/201601643-4533a678-e4ef-4687-b741-7db20b277a22.png)
![image](https://user-images.githubusercontent.com/111613205/201601671-c93eb1a6-0ad1-4fef-9a16-645bf239c340.png)


 

 
The cert folder displays the TLS certificates that are created.
 
 ![image](https://user-images.githubusercontent.com/111613205/201601696-7a9db21b-8716-4c14-ac3e-934fe83730a7.png)
![image](https://user-images.githubusercontent.com/111613205/201601708-ffc61569-eaa1-4885-aad7-30cea4c6c367.png)
![image](https://user-images.githubusercontent.com/111613205/201601732-a24b1e43-97b8-4eb2-8eb9-281c5a4c5356.png)
![image](https://user-images.githubusercontent.com/111613205/201601754-517b4667-e958-4b3e-8fb8-9b6a66d01dec.png)
![image](https://user-images.githubusercontent.com/111613205/201601771-86e023ae-a1c2-4c25-9939-8cd911e6aa66.png)


 
 

 
Now, for the TLS certificate which is generated we have to configure Tomcat to work with SSL. Therefore, before that we would have to generate a keystore from root and all the way until server.
Addition of Root certificate and the Signing certificate
Use the below commands to for adding the root certificate and the signing certificate.
1)	keytool -import -alias root-ca -keystore certs/simple.jks -trustcacerts -file ca/root-ca.crt
2)	keytool -import -alias signing-ca -keystore certs/simple.jks -trustcacerts -file ca/signing-ca.crt
 
![image](https://user-images.githubusercontent.com/111613205/201601820-4e79356a-e6af-4591-9f51-5cbcf8473ce6.png)
![image](https://user-images.githubusercontent.com/111613205/201601852-9a5059ad-587b-47f6-9f04-d2f254417cf5.png)

 


Upon the configuration changes restart Tomcat and view the server.
![image](https://user-images.githubusercontent.com/111613205/201601877-5601d0f5-5178-4585-8522-573bbcbe69ff.png)

 

References:
•	https://pki-tutorial.readthedocs.io/en/latest/simple/
•	https://tomcat.apache.org/tomcat-7.0-doc/ssl-howto.html

