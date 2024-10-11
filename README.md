
# PRÁCTICA 3

## Consulta DNS de danielcastelao.org

Realiza unha consulta co comando `dig danielcastelao.org` e identifica cada parte da resposta (IN, CNAME, A, QUERY SECTION, ANSWER SECTION, AUTHORITY SECTION, etc.).

### Tipo de consulta

Realicei unha consulta tipo **Address Record**. Esta devolve a dirección IP IPv4 asociada a un dominio.

### Descrición das partes da resposta

1. **Header:**
   - **opcode:** Query: tipo de operación, neste caso unha consulta.
   - **status:** NOERROR: Non houbo erros ao realizar a consulta.
   - **id:** Identificador da consulta.
   - **flags:** Indica o estado da consulta, como `qr` (resposta), `rd` (recursión solicitada) e `ra` (recursión dispoñible).

2. **Question Section:** Mostra o que se está consultando. 
   - **danielcastelao.org:** Dominio consultado.
   - **IN:** Clase da consulta, sempre IN (INTERNET).
   - **A:** Tipo de rexistro, neste caso solicítase o rexistro A (IPv4).

3. **Answer Section:** Mostra a resposta.
   - **danielcastelao.org:** Dominio consultado.
   - **900:** Tempo de vida (TTL) do rexistro en segundos, indicando canto tempo debe almacenarse en caché.
   - **IN:** Clase de resposta (sempre internet).
   - **178.211.133.37:** Dirección IP asociada a danielcastelao.org.

4. **Authority Section:**
   - Indica os servidores DNS autorizados para a zona do dominio.
   - **danielcastelao.org:** Dominio consultado.
   - **172800:** TTL en segundos.
   - **NS:** Tipo de rexistro, que indica servidores de nomes (Name Service).

5. **OPT Pseudosection:**
   - Información sobre o uso de extensións DNS (EDNS) e o tamaño máximo de mensaxe UDP soportado.

6. **Query time:** Tempo que tardou a consulta.
7. **SERVER:** Dirección IP e porto do servidor DNS que respondeu á consulta.
8. **WHEN:** Data e hora na que se realizou a consulta.

### Consultas adicionais

**Realiza consultas dos seguintes nomes e identifica as diferenzas: `moodle.danielcastelao.org`, `www.danielcastelao.org`.**

Ao realizar as consultas `dig` para `moodle.danielcastelao.org` e `www.danielcastelao.org`, atopáronse as seguintes diferenzas:

1. **Dominio Consultado:** Cada subdominio é diferente (`moodle` e `www`).
2. **Sección de Resposta:** 
   - `moodle.danielcastelao.org` resolve a `82.98.134.234`.
   - `www.danielcastelao.org` resolve a `82.98.134.235`.
3. **Tempo de Consulta:** 
   - `moodle` tardou `50 ms`, e `www` tardou `55 ms`.
4. **Sección de Autoridade:** Ambos comparten os mesmos servidores DNS (`ns1.dondominio.com` e `ns2.dondominio.com`).

---

## Consulta de servidores DNS autoritativos

Averigua o nome e IP dos servidores DNS autoritativos de `www.danielcastelao.org`. ¿Por que adoitan ser 2 servidores autoritativos?

Utilicei o comando:
```bash
dig ns www.danielcastelao.org
```
Só me apareceu un servidor autoritativo. A súa IP é `8.8.8.8 #53` e o seu nome é `danielcastelao.org`. A principal razón de ter dous é garantir redundancia e dispoñibilidade entre servidores.

---

## Consultas inversas

Realiza as consultas de nomes inversos para `130.206.164.68` e outras dúas IPs que se che ocurran.

Para realizar consultas inversas de DNS, utilizamos o comando `dig` xunto cunha consulta do tipo `PTR`. Isto permite resolver a dirección IP e obter o nome de dominio asociado.

Aquí están os comandos para as IPs:

1. **Consulta inversa para 130.206.164.68:**
   ```bash
   dig -x 130.206.164.68
   ```

2. **Consulta inversa para unha IP adicional (por exemplo, 8.8.8.8):**
   ```bash
   dig -x 8.8.8.8
   ```

3. **Consulta inversa para outra IP (por exemplo, 1.1.1.1):**
   ```bash
   dig -x 1.1.1.1
   ```

---

## Cambiar o servidor DNS

### ¿A que servidor DNS estás consultando? ¿Como o podes cambiar sen tocar os ficheiros de configuración do sistema?

Para comprobar que servidor DNS estás utilizando, executa:
```bash
dig 
```
Con este comando, verás que estás consultando o servidor `8.8.8.8`.

Para cambiar o servidor DNS de forma temporal e realizar consultas DNS con `dig`, podes especificar o servidor DNS directamente no comando sen tocar a configuración do sistema. Aquí tes como facelo:

### Usar `dig` cun servidor DNS específico

Para consultar un servidor DNS específico (como o de Google `8.8.8.8`), engade a dirección do servidor utilizando o símbolo `@` no comando.

Por exemplo, para consultar o dominio `google.com` utilizando o servidor DNS de Google (`8.8.8.8`):
```bash
dig @8.8.8.8 google.com
```

Este comando realiza a consulta DNS directamente ao servidor `8.8.8.8` sen cambiar a configuración DNS predeterminada do sistema.

---

## Exemplos adicionais con `dig`

1. **Consulta de rexistros MX (servidores de correo) de `example.com` usando Cloudflare (`1.1.1.1`):**
   ```bash
   dig @1.1.1.1 example.com MX
   ```

2. **Consulta de servidores de nomes (NS) de `example.com` usando o servidor DNS de OpenDNS (`208.67.222.222`):**
   ```bash
   dig @208.67.222.222 example.com NS
   ```

3. **Consulta inversa usando o servidor de Google:**
   ```bash
   dig @8.8.8.8 -x 142.250.184.206
   ```

---

## Obter o rexistro SOA do dominio

### Obtén o rexistro SOA (Start of Authority) do dominio `moodle.danielcastelao.org`.

Para obter o rexistro SOA usando `dig`, primeiro consultaré o servidor DNS de Google e logo ao servidor DNS primario do dominio `danielcastelao.org`.

### 1. Consultar o rexistro SOA ao servidor DNS de Google (`8.8.8.8`):

```bash
dig @8.8.8.8 moodle.danielcastelao.org SOA
```

### 2. Consultar o servidor primario do dominio `danielcastelao.org`:

Executa o seguinte comando para obter os servidores de nomes (NS) autorizados para o dominio:

```bash
dig @8.8.8.8 danielcastelao.org NS
```

### 3. Consultar o rexistro SOA ao servidor DNS primario:

Unha vez identificado o servidor DNS primario, consulta directamente a ese servidor. Por exemplo, se o servidor primario resultante é `ns1.dominio.com`, a consulta sería:

```bash
dig @ns1.dominio.com moodle.danielcastelao.org SOA
```

---

## Consulta de IP e TTL de www.elpais.com

### Consulta a IP de `www.elpais.com`.

Para obter a IP e o TTL do rexistro usando `dig`, executa o seguinte comando:

```bash
dig www.elpais.com
```

O resultado incluirá unha sección como esta:
```
;; ANSWER SECTION:
www.elpais.com.    60    IN    A    199.232.198.133
```

Neste exemplo, o **TTL** é de **60 segundos**, indicando canto tempo se manterá en caché antes de volver consultar o servidor DNS autoritativo.

### Observación do cambio do TTL:

Se executas o comando de novo antes de que pase o TTL completo, verás que o valor do TTL diminúe. Por exemplo, se consultas despois de 30 segundos, o TTL podería estar preto de 30.

---

## Determinación do TTL máximo

### Determina o TTL máximo (orixinal) dun nome de dominio.

Para obter o TTL máximo dun dominio, consulta o rexistro SOA:

```bash
dig www.google.es SOA
```

Busca a sección de resposta (`ANSWER SECTION`) onde se mostrará o rexistro SOA, similar a isto:

```
;; ANSWER SECTION:
google.es.      3600    IN      SOA     ns1.google.com. dns-admin.google.com. 1234567890 7200 1800 1209600 300
```

Neste caso, o valor `300`