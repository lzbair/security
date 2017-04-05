### To init CA infra: 

1- Go to ./authority/
2- Update caconfig.cnf with CA setting
3- Execute ./ca

### To apply for CRS:
1- Go to ./applicant
2- Update request.cnf with applicant setting
3- Execute ./apply

### To issue and sign a certificate
1- Go to ./authority
2- Execute ./certify
3- Check your-cert.pem
4- Check ./authority/signedcerts/ for signed certificate
