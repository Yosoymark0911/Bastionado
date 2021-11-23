$DIR = ${$HOME}/ca/easy-rsa
=============================conf-easy-rsa.sh====================================
mkdir -R $DIR
ln -s /usr/share/easy-rsa/* $DIR
$DIR init-pki 
echo '
set_var EASYRSA_REQ_COUNTRY    "ES"
set_var EASYRSA_REQ_PROVINCE   "Castellón"
set_var EASYRSA_REQ_CITY               "Castellón"
set_var EASYRSA_REQ_ORG                "IESElCaminas"
set_var EASYRSA_REQ_EMAIL            "juancarlos.requena@ieselcaminas.org"
set_var EASYRSA_REQ_OU                  "Dep-Informatica"
set_var EASYRSA_ALGO                        "ec"
set_var EASYRSA_DIGEST                     "sha512"
' > $DIR/vars
$DIR/easyrsa build-ca

#Extraer certificado ca.crt a el equipo cliente con scp 
#Cliente
sudo cp /home/jc/tools/ca.crt /usr/local/share/ca-certificates
sudo update-ca-certificates
=================================================================================
=============================firma.sh============================================
mkidr prueba-csr
openssl genrsa -out orion-server.key 
openssl req -new -key orion-server.key -out  orion-server.req
scp orion-server.req administrador@[IP]:$HOME/orion-server.req
#Servidor
$DIR/easyrsa import-req ../../orion-server.req orion-server
$DIR/easyrsa sign-req server sammy-server
=================================================================================
=============================revocar.sh==========================================
$DIR/easyrsa revoke orion-server
$DIR/easyrsa gen-crl
scp $DIR/pki/crl.pem administrador@[IP]:/tmp
#verificar
openssl crl -in $DIR/pki/crl.pem -noout -text
openssl crl -in /tmp/crl.pem -noout -text
=================================================================================
