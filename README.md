# Funcx demo

- Referência tutorial: [Quickstart](https://funcx.readthedocs.io/en/latest/quickstart.html) 

## Pré-requisitos
1. python 3.8+ 
2. pip

- Verifique a versão do python
```console
$ python3 --version
Python 3.11.5
```

- Verifique se o cliente tem acesso a internet e consegue se conectar ao Globus Compute Service
```console
$ curl https://compute.api.globus.org/v2/version
"1.2.0"
```

#### Instale o Globus Compute Client

- Configure um ambiente virtual
```console
$ python3 -m venv venv
$ source venv/bin/activate
```

- Instale o Globus Compute Client
```console
$ pip install globus-compute-sdk
```

#### Instale o Globus Compute Endpoint
```console
$ pip install globus-compute-endpoint
```

### Primeira Execução
```console
$ python3
>>> from globus_compute_sdk import Client
>>> Client()
Please authenticate with Globus here:
------------------------------------
https://auth.globus.org/v2/oauth2/authorize?[...very...long...url]&prompt=login
------------------------------------

Enter the resulting Authorization Code here:
```
Só é necessário necessário executar uma vez para configurar as credenciais


#### Configure o endpoint
```console
$ globus-compute-endpoint configure
$ globus-compute-endpoint start default
```
- Documentação sobre configuração de endpoints: [docs](https://funcx.readthedocs.io/en/latest/endpoints.html) 

- O endpoint default é o local
- Para recuperar o endpoint ID, execute:

```console
$ globus-compute-endpoint list
+--------------------------------------+---------+---------------+
|             Endpoint ID              | Status  | Endpoint Name |
+======================================+=========+===============+
| 00db3b40-29e2-4a7a-867e-bbd0ade3f7dc | Running | default       |
+--------------------------------------+---------+---------------+
```


### Executando uma Função
- Crie um arquivo .py com o seguinte código:
```python
from globus_compute_sdk import Executor

# First, define the function ...
def add_func(a, b):
    return a + b

tutorial_endpoint_id = '00db3b40-29e2-4a7a-867e-bbd0ade3f7dc' # Seu endpoint ID default
# ... then create the executor, ...
with Executor(endpoint_id=tutorial_endpoint_id) as gce:
    # ... then submit for execution, ...
    future = gce.submit(add_func, 5, 10)

    # ... and finally, wait for the result
    print(future.result())
```

- Execute o arquivo .py com o seguinte comando:
```console
$ python3 <nomeDoArquivo>.py
15
```
