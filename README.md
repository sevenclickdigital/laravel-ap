# Laravel  -

Versão do Laravel: Laravel Framework 10.40.0

Este projeto utiliza Laravel Sail para execução local.

## Instalação

```bash
docker run --rm \
    -u "$(id -u):$(id -g)" \
    -v "$(pwd):/var/www/html" \
    -w /var/www/html \
    laravelsail/php83-composer:latest \
    composer install --ignore-platform-reqsInicie os containers:
```
Inicie os containers:
``` bash
sail up -d
```
Rode o composer novamente:
``` bash
sail composer install
```
Gere a chave da aplicação:
``` bash
sail artisan key:generate
```
Execute as migrações e popule o banco de dados:
``` bash
sail artisan migrate:fresh --seed
```
Instale as dependências JavaScript:
``` bash
sail npm install
```
Compile os assets:
``` bash
sail npm run build
```

Consulte a documentação do Sail para mais detalhes sobre a execução do projeto.
## Autenticação
Sanctum

## Filas

Este projeto está configurado para rodar filas no Redis.

Para processar a fila de e-mails:

``` bash
sail artisan queue:work --queue=email
```

Para processar a fila de geração de Cartão Emergência:

``` bash
sail artisan queue:work --queue=qr_code_generation
```

Limpar todas as filas:

``` bash
sail artisan queue:flush
```

## Ferramentas

### Laravel husky
Este projeto incorpora ferramentas essenciais para manutenção e qualidade do código:

- Laravel Pint: Utilizado para aprimorar a formatação do código, garantindo uma estrutura clara e consistente.

- Larastan: Empregado para realizar análises estáticas, identificando potenciais problemas no código antes mesmo da execução.

- Pest: Empregado para realizar testes automatizados, garantindo a integridade e funcionalidade do código.

Essas ferramentas são executadas de forma automatizada durante o processo de commit, integrando-se ao Pest para evitar a inclusão de possíveis erros na branch principal. Consulte a  [Documentação](https://typicode.github.io/husky/) para obter mais detalhes sobre a configuração dessas integrações.

### Laravel Pint

Para formatar o código antes de commitar:

``` bash
sail pint
```

### Laravel Stan (Larastan)

Execute o Laravel Stan com:

``` bash
sail php ./vendor/bin/phpstan
```
### Testes

Para rodar todos os testes:

``` bash
sail pest
```

Para rodar um teste específico:

``` bash
sail pest tests/Feature/Api/Address/AddressTest.php
```

Criar um novo teste automatico
``` bash
sail artisan module:test NomedaModel
```

### Command

O comando `module:all` é uma ferramenta poderosa para agilizar o processo de criação de diversos componentes deste projeto. Ele automatiza a geração de vários artefatos relacionados a um modelo, facilitando o desenvolvimento e seguindo as convenções do projeto.

#### Funcionalidades Principais:

**Criação de Componentes:**
O comando `module:all` permite a criação dos seguintes componentes para um modelo específico:
- Model
- Observer
- Policy
- Controller
- Request
- Repository
- Resource
- Test

#### Uso do Comando:

O comando `module:all` aceita a seguinte opção:

- `--model=nome_do_modelo`: Especifica o nome do modelo para o qual os componentes serão gerados.

exemplo de uso
``` bash
sail artisan module:all --model=CoronaVaccination
```

``` php
// apenas para referencias para criar os modulos
// DummyModel -> AdvanceDirective
// CamelObject -> advanceDirective
// DummyModelPluralObject -> advancedirectives
// DummyModelObject -> advancedirective
```

#### Lembretes:

- No arquivo `routes\api.php`, é necessário adicionar uma rota de recursos para o modelo criado.
- No método `boot` do arquivo `app\Providers\AppServiceProvider.php`, é necessário registrar o observer para o modelo criado.

Este comando automatizado simplifica o processo de criação e configuração de componentes em projetos, aumentando a produtividade.

### Localize
Este projeto utiliza o `Localize` para gerenciamento de traduções. Certifique-se de manter os arquivos de idioma atualizados usando as ferramentas fornecidas pelo Localize. [Documentação](https://github.com/amiranagram/localizator#remove-missing-keys)

``` bash
sail artisan localize de,en,pt-br
```
> Nota: As strings que você já traduziu não serão substituídas.

Remover chaves ausentes
``` bash
php artisan localize --remove-missing
```


### Telescope

O Telescope está disponível em http://localhost/telescope.

### Horizon

Inicie o Horizon com:

``` bash
sail artisan horizon
```

O Horizon estará acessível em http://localhost/horizon.

### Logs
Para facilitar a depuração do aplicativo, é altamente recomendável utilizar logs e canais de log. Os logs podem fornecer informações valiosas sobre o comportamento do sistema, ajudando na identificação e resolução de problemas.

Podemos configurar canais específicos de log para diferentes tipos de mensagens.

Temos um canal chamado 'discord' que é usado para disparar alertas sobre problemas críticos que exigem atenção imediata. Este canal deve ser usado apenas para alertas e não para exibir erros diretamente.

O objetivo é notificar os desenvolvedores para que eles possam verificar os detalhes do erro nos logs.

Segue um exemplo de como usar o canal 'discord' para disparar um alerta:

```php
use Illuminate\Support\Facades\Log;

Log::channel('discord')->warning("Ocorreu uma falha na API do QR code. \nVerifique os logs (generateQrCode.log) para mais detalhes.");
```

> Temos disponíveis instalada uma ferramenta para facilitar a visualização dos logs. Para acessar, use http://localhost/v1/dev/logs


## Padrão de Commits

Para manter a consistência no versionamento do código, é crucial seguir o seguinte padrão para commits e branches:

-   **Branches:** Nomeie suas branches seguindo o formato `feature/TASK-nome-da-tarefa`.

    Exemplo: `feature/TASK-translation-of-document-responses`


As novas features devem ser sempre criadas a partir da branch `develop`. Isso ajuda a organizar o desenvolvimento de novas funcionalidades de maneira estruturada e a manter a integridade do fluxo de trabalho.




## Observações do projeto

O `ProfileMiddleware` foi implementado para garantir que o ID do perfil esteja disponível globalmente em todas as partes do aplicativo. Este middleware é responsável por verificar se o usuário está autenticado e se o perfil associado a ele está carregado corretamente. Em seguida, ele injeta o ID do perfil no request, permitindo que seja acessado em diferentes partes do sistema, conforme necessário.

Para acessar o ID do perfil em qualquer parte do aplicativo, basta recuperar o valor do request usando a chave `profile_logged`.
