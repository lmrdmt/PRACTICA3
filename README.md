# PRACTICA3 

    Realiza unha consulta "dig danielcastelao.org" e identific cada parte da resposta (IN, CNAME, A, QUERY SECTION, ANSWER SECTION, AUTHORITY SECTION, etc)
    
    Fixen unha consulta tipo **Address Record** ; Ésta devolve a dirección IP IPV4 asociada cun dominio.

    A continuación explicaranse as distintas partes:
        Header:
            - opcode: Query : tipo de operación, neste caso unha consulta tipo (query)
            - status: NOERROR: Non houbo erros ao realizar a consulta
            - id: Identificador da consulta
            - flags: Indica o estado da consulta, como qr(resposta),rd (recursión solicitada) e ra (recursión disponible)
        
        Question Section: Móstrase o que se está consultar. Neste caso 
            - daniecastelao.org: O dominio consultado.
            - IN: Clase da consulta, sempre IN (INTERNET).
            - A: Tipo de rexistro neste caso estas solicitar o rexistro A (ipv4).

        Answer Section: Móstrase a resposta. 
            - danielcastelao.org: Dominio consultado
            - 900: tempo de vida (TTL) do rexistro en segundos, o que indica canto tempo debe almacenarse a caché en resposta.
            - IN: Clase de resposta (sempre internet)
            - 178.211.133.37: Dirección IP asociada a danielcastelao.org
        Authority Section:
            - Indica os servidores DNS autorizados para a zona de dominio
            - danielcastelao.org:Dominio consultado.
            - 172800: TTL en segundos.
            - NS: Tipo de rexistro, que indica servidores de nomes (Name Service)
        OPT Pseudosection:
            -Indormación sobre o uso de extensións DNS (EDNS) e o tamaño máximo de mensaxe UDP soportado.
        Query time: Tempo que tardó a consulta.
        SERVER: Dirección iP e porto do servidor DNS que respondeu á consulta
        WHEN: Fecha e hora na que se realizou a consulta.
    
    **Realiza consutas dos seguintes nomes e identifica as diferencias: moodle.danielcastelao.org, www.danielcastelao.org  **
     

---

Ao realizar as consultas `dig` para `moodle.danielcastelao.org` e `www.danielcastelao.org`, atopáronse as seguintes diferenzas:

1. **Dominio Consultado**: Cada subdominio é diferente (`moodle` e `www`).

2. **Sección de Resposta**: 
   - `moodle.danielcastelao.org` resolve a 82.98.134.234.
   - `www.danielcastelao.org` resolve a 82.98.134.235.

3. **Tempo de Consulta**: 
   - `moodle` tardou 50 ms, e `www` tardou 55 ms.

4. **Sección de Autoridade**: Ambos comparten os mesmos servidores DNS (`ns1.dondominio.com` e `ns2.dondominio.com`).


    
    Averigua o nome e IP dos servidores de DNS autoritativos de www.danielcastelao.org, por qué soen ser 2 servidores autoritativos?
    Utilcei o comando 
    ´´´bash
    dig ns www.danielcastelao.org
    ´´´ 
    Só apareceume un servidor autoritativo. A súa Ip é a 8.8.8.8 #53 e o seu nome é danielcastelao.org.
    A principal razón de ter dous é para garantir redundancia e dispoñibilidade entre servidores.

    Realiza as consultas de nomes inversas: 130.206.164.68 e de outras dúas IPs que se che ocorran.

    Para realizar consultas inversas de DNS, utilizamos o comando `dig` xunto cunha consulta do tipo `PTR`. Isto permite resolver a dirección IP e obter o nome de dominio asociado.

Aquí está como realizarías as consultas para as IPs:

1. **Consulta inversa para 130.206.164.68**:
   ```bash
   dig -x 130.206.164.68
   ```

2. **Consulta inversa para unha IP adicional, por exemplo, 8.8.8.8 (servidor DNS de Google)**:
   ```bash
   dig -x 8.8.8.8
   ```

3. **Consulta inversa para outra IP, por exemplo, 1.1.1.1 (servidor DNS de Cloudflare)**:
   ```bash
   dig -x 1.1.1.1
   ```

    A qué servidor DNS estás consultando? Cómo o podes cambiar sen tocar os ficheiros de configuración do sistema?

    Obtén o rexistro SOA (Start of Authority) do dominio  moodle.danielcastelao.org preguntándolle ó servidor DNS de google e logo preoguntándollo directamente ó servidor primario do dominio danielcastelao.org. 

    Consulta a IP de www.elpais.com. Cánto tempo queda almaceado o rexistro de recurso no DNS local?, se preguntas ó DNS local por este recurso, qué observas no TTL do rexistro?
    Busca o TTL de distintos nomes de dominio de servicios que escollas, a qué se poden deber as diferencias?

    Determina o TTL máximo (original) dun nome de dominio.
    Averigua cántas máquinas con distintas IPs están detrás do dominio web www.google.es, sempre son as mesmas e na mesma orde? por qué?

    Pregunta o mesmo a un server raiz (J.ROOTSERVERS.NET por exemplo) e comproba na resposta se o server acepta o modo recursivo
    Se queremos ver tóda-las queries que fai o servidor de DNS, qué opción temos que usar? averigua a IP de www.timesonline.co.uk, especifica os pasos dados
     Usando a información dispoñible a traveso do DNS especifica a máquina (nome e IP) ou máquinas que actúan como servers de correo do dominio danielcastelao.org
    Podes obter os rexistros AAAA de www.facebook.com? a qué corresponden?

