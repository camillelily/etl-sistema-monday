etl-sistema-monday

ETL em Google Apps Script que concilia a carteira de clientes por gerente entre o sistema interno (Q Bem) e o Monday.com, identificando divergências de nome, CNPJ e gerente responsável.

Como funciona
O processo é dividido em 3 automações, cada uma rodando em um Google Apps Script separado:

1. extract-qbem/
Faz login no qbem, percorre os "Perfis de Dados" (um por gerente) e extrai as empresas (estipulantes) ativas de cada perfil, incluindo CNPJ. Escreve o resultado em uma planilha (qbem), uma aba por gerente.

2. extract-monday/
Consulta o board do Monday.com via API, lê os grupos (um por gerente) e extrai as empresas da carteira de cada um (status ativa/inativa, estipulante, apólice, CNPJ). Escreve o resultado em uma planilha (monday).

3. compare/ — cruzarDiferencas()
Lê as duas planilhas geradas nas etapas anteriores e cruza os registros:
Normaliza nome (maiúsculas, remove sufixos numéricos) e CNPJ (só dígitos) antes de comparar
Detecta sufixos que não batem com o próprio CNPJ (inconsistência de cadastro no Q Bem)
Gera uma aba DIFERENÇAS na planilha de resultado, dividida em:
Qbem sem correspondente no Monday
Monday sem correspondente no Q bem

Essas divergências são usadas como checklist de correção manual nos dois sistemas (inclusive diferenças só de acentuação/pontuação no nome, que não são filtradas) Para indentificar padrões de erros dos usuários e pontos de melhora.

Observações

Login no Q bem é feito via POST em /Autenticacao.

IDs de planilha, board do Monday e credenciais ficam fora do código-fonte (ver .gitignore).
