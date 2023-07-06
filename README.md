# Bucket Stream

**Find interesting Amazon S3 Buckets by watching certificate transparency logs.**

This tool simply listens to various certificate transparency logs (via certstream) and attempts to find public S3 buckets from permutations of the certificates domain name.

![Demo](https://i.imgur.com/ZFkIYhD.jpg)

Seja responsável. Eu criei esta ferramenta principalmente para destacar os riscos associados aos Buckets públicos do S3 e para dar um toque diferente aos ataques baseados em dicionário habituais. Algumas dicas rápidas se você usar Buckets S3:

Randomize os nomes dos seus Buckets! Não há necessidade de usar empresa-backup.s3.amazonaws.com.
Defina permissões apropriadas e faça auditorias regularmente. Se possível, crie dois Buckets - um para seus ativos públicos e outro para dados privados.
Esteja atento aos seus dados. O que os fornecedores, empreiteiros e terceiros estão fazendo com eles? Onde e como eles estão armazenados? Essas questões básicas devem ser abordadas em cada política de segurança da informação.
Experimente o Amazon Macie - ele pode classificar e proteger automaticamente dados confidenciais.
Agradeço ao meu bom amigo David (@riskobscurity) pela ideia.

Instalação
Python 3.4+ e pip3 são necessários. Então, simplesmente:

git clone https://github.com/HttpEduardo/FindAmazon.git
(opcional) Crie um ambiente virtual com pip3 install virtualenv && virtualenv .virtualenv && source .virtualenv/bin/activate
pip3 install -r requirements.txt

## Uso

Simplesmente execute `python3 bucket-stream.py`.

Se você fornecer acesso à AWS e chaves secretas em `config.yaml`, o Bucket Stream tentará acessar os buckets autenticados e identificar o proprietário dos buckets. **Usuários não autenticados têm taxa severamente limitada.**

    uso: python bucket-stream.py

    Encontre baldes interessantes do Amazon S3 observando os logs de transparência do certificado.

    argumentos opcionais:
      -h, --help Mostra esta mensagem de ajuda e sai
      --only-interesting Registra apenas buckets 'interessantes' cujos conteúdos correspondem
                            qualquer coisa dentro de keywords.txt (padrão: False)
      --skip-lets-encrypt Skip certs (e, portanto, domínios listados) emitidos por Let's
                            Criptografar CA (padrão: Falso)
      -t , --threads Número de encadeamentos a serem gerados. Mais fios = mais poder.
                            Limitado a 5 threads se não autenticado.
                            (padrão: 20)
      --ignore-rate-limiting
                            Se você ignorar os limites de taxa, nem todos os baldes serão
                            verificado (padrão: Falso)
      -l, --log Registra os buckets encontrados em um arquivo buckets.log (padrão:
                            Falso)
      -s, --source Fonte de dados para verificar as permutações de bucket. Usos
                            logs de transparência de certificado se não for especificado.
                            (padrão: Nenhum)
      -p, --permutations Caminho do arquivo contendo uma lista de permutações para tentar
                            (veja permutações/dir). (padrão: permutações\padrão.txt)

## F.A.Qs

- **Nada parece estar acontecendo**

   Paciência! Às vezes, os logs de transparência de certificado podem ficar silenciosos por alguns minutos. O ideal é fornecer os segredos da AWS em `config.yaml`, pois isso acelera muito a taxa de verificação.

- **Encontrei algo altamente confidencial**

   **Denunciem** - por favor! Geralmente, você pode descobrir o proprietário pelo nome do balde ou fazendo um reconhecimento rápido. Caso contrário, entre em contato com as equipes de suporte da Amazon.

