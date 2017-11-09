# winicpdemos

IBM Cloud Private Demo on SG ICC

Install shrewsoft
Get the VPN connection
Update your hosts file
192.168.64.xxx	api.mgmt.cf.sgcc.demo.lan login.mgmt.cf.sgcc.demo.lan doppler.mgmt.cf.sgcc.demo.lan asean-discovery-news-demo.apps.cf.sgcc.demo.lan test-asean-discovery-news-demo.apps.cf.sgcc.demo.lan ng4-demo.apps.cf.sgcc.demo.lan 192.168.64.xxx cfng4.apps.cf.sgcc.demo.lan 192.168.64.xxx ngx-admin.apps.cf.sgcc.demo.lan

login to CF on ICP cf api https://api.mgmt.cf.sgcc.demo.lan --skip-ssl-validation email: cfadmin password: xxxxxxxx

Download sample cf code

https://github.com/cloudfoundry-samples/cf-sample-app-nodejs

cf push

open the app on ICP

Kubernetes

Install kubectl https://v1-7.docs.kubernetes.io/docs/tasks/tools/install-kubectl/

Install ICP CLI https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0/manage_cluster/install_cli.html

Login to ICP Cluster bx pr login -a https://<master_ip_address>:8443 --skip-ssl-validation

Create client certificate

sudo vi /etc/docker/certs.d/mycluster.icp:8500/ca.crt

-----BEGIN CERTIFICATE----- MIIDBTCCAe2gAwIBAgIJAPmcQ98f9IWQMA0GCSqGSIb3DQEBCwUAMBgxFjAUBgNV BAMMDW15Y2x1c3Rlci5pY3AwIBcNMTcxMTA2MDQyMDI1WhgPMjExNzEwMTMwNDIw MjVaMBgxFjAUBgNVBAMMDW15Y2x1c3Rlci5pY3AwggEiMA0GCSqGSIb3DQEBAQUA A4IBDwAwggEKAoIBAQDSyz8DGrIZhHgrnUSKSBIIDdoxFF5i0CO2PIJ8gB6tgCXo 0qqumVVwWX0SUdTEceUZqT8lCvxhZAyQXrjyKcccn3EZ/nZAs/ybmOiuMbT2MTGS Km1YYg8atOrWNopoIvR5CgqM6oOmABnUvtWFKHz+0SkV5SDsqApl2KLQvGzXkQvY FBvPbVB0mTgGrp1k3qorANN1D/SL4khOXUcD0XjxtR6Nes3ve+PJMz3FRzFt3k9g JJ9y7NWPDXlmMpPXsS01eyWYJMdj0wdSNatUo4r0r09HgxzPIjcfA8pCC8PcFH3e p/EX7AteFg8gxiW/H4EFazH00HYmULlGC0sppro7AgMBAAGjUDBOMB0GA1UdDgQW BBSUhsnLMdX5Asnsp9bGQ32yts5IzzAfBgNVHSMEGDAWgBSUhsnLMdX5Asnsp9bG Q32yts5IzzAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBCwUAA4IBAQC0yqRippEu 9538HbKKgBCVZ7OrjkGeUHIYSSw+Ebdlni0hn9p64D90AXsVECuuzSr7jfBP49v7 u/5jO+MhWj3SixSA1L/Vt4iIKKJd9Pm34oPFhjJ0Xglq71TLdi6Zt91o7fZrXRwl IIKK5+17o9SczIPYcknhGR9COT94laVmwDNR6l/GiZGCguqBqsCNFO71CskwymnH 6gEDUJ4G21AglHRErlU3rbZpVJImLq2gvjE2L9Xx5nwIlx1WSTy4DekU4QSNgryp nTu0scu+AhUcsUImbbIbFgMu9Q1xnOeIgdX4ZU9U3Hj9vi8AXO7NEm5yIY2UjftR FD8mlVEh5Div -----END CERTIFICATE-----

Kubectl cheatsheet: 

kubectl create namespace [space name]

Certificate

Create container pod mysql-client

kubectl run -it --rm --image=mysql:5.6 --restart=Never mysql-client -- mysql -h mysql -ppassword

Accessing running pod

kubectl exec -it mysql-client -- /bin/bash kubectl exec -it mysql-client -- mysql -h mysql -p password

Expose service

kubectl expose deployment hello-world --type=LoadBalancer --name=my-service

Git global setup

git config --global user.name "Winton"
git config --global user.email "wintonjkt@gmail.com"

Create a new repository

git clone http://prd-master1/Winton/sprdemo.git
cd sprdemo
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master

Existing folder

cd existing_folder
git init
git remote add origin http://prd-master1/Winton/sprdemo.git
git add .
git commit -m "Initial commit"
git push -u origin master

Existing Git repository

cd existing_repo
git remote add origin http://prd-master1/Winton/sprdemo.git
git push -u origin --all
git push -u origin --tags
