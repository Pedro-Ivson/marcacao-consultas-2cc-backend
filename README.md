# Marcação de Consultas - Backend

API didática da clínica para as aulas de 08/10 e 15/10/2026. Usa Node.js, TypeScript, Express e o SQLite nativo do Node. O app Expo permanece no repositório separado e não é alterado nesta etapa.

## Requisitos

- Node.js 22.13.0 ou mais recente. A versão 22.13 removeu a necessidade da flag experimental do `node:sqlite`.
- npm

## Instalação e execução

```bash
npm install
npm start
```

A API inicia em `http://localhost:3000`. Para reiniciar automaticamente ao editar os arquivos:

```bash
npm run dev
```

O banco versionado `src/dados/clinica.db` contém o catálogo e a consulta de exemplo da aula de 15/10/2026. Se o arquivo não existir, o primeiro início cria o banco e semeia o catálogo de `src/dados/catalogo.json` quando a tabela `especialidades` está vazia. O banco é versionado; os arquivos auxiliares `*.db-wal` e `*.db-shm` continuam ignorados.

## Rotas

| Método | Caminho | Resultado |
| --- | --- | --- |
| GET | `/clinica` | Estado do serviço |
| GET | `/especialidades` | Catálogo de especialidades |
| GET | `/medicos` | Médicos com objeto `especialidade` |
| GET | `/usuarios` | Usuários sem o campo `senha` |
| GET | `/consultas` | Lista de consultas; lista vazia retorna `200` e `[]` |
| GET | `/consultas/:id` | Consulta pelo id, `400` para id inválido ou `404` para id inexistente |
| POST | `/consultas` | Cria consulta e retorna `201` |

O POST recebe JSON com ids numéricos positivos e data civil no formato `YYYY-MM-DD`:

```json
{
  "medicoId": 1,
  "pacienteId": 1,
  "data": "2026-10-20",
  "observacoes": "Retorno via API"
}
```

A data deve ser a partir de amanhã e até dois meses à frente. O médico precisa existir e estar ativo; o usuário indicado precisa ter o papel `paciente`. Erros de validação retornam `400`, ids inexistentes retornam `404`, e JSON malformado retorna `400` em JSON.

Esta etapa não implementa login, JWT, escrita de cadastro nem alterações administrativas. As credenciais de `catalogo.json` são dados fictícios para o laboratório e não devem ser reutilizadas em produção.

## Verificação local

```bash
npm run typecheck
```

Depois, teste os GETs e o POST em `http://localhost:3000` pelo Postman. Não é necessário enviar Authorization nesta aula.

Para agilizar a conferência, importe `postman/Clinica-2CCPB.postman_collection.json` no Postman. A collection está organizada nas cinco pastas da aula e calcula as datas de teste no momento de cada envio; execute os requests na ordem apresentada. O banco versionado já contém a consulta de exemplo com data `2026-11-03`; em um banco vazio, GET `/consultas` retorna `200` com `[]`. Depois do POST válido, a lista inclui também a consulta criada, e a collection guarda o id retornado para o GET seguinte.
