# Marcação de Consultas - Backend

API didática da clínica para a aula de 08/10/2026. Usa Node.js, TypeScript, Express e o SQLite nativo do Node. O app Expo permanece no repositório separado e não é alterado nesta etapa.

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

O banco `src/dados/clinica.db` é criado no primeiro início. O catálogo em `src/dados/catalogo.json` é semeado quando a tabela `especialidades` está vazia. O banco e os arquivos auxiliares do SQLite são ignorados pelo Git.

## Rotas

| Método | Caminho | Resultado |
| --- | --- | --- |
| GET | `/clinica` | Estado do serviço |
| GET | `/especialidades` | Catálogo de especialidades |
| GET | `/medicos` | Médicos com objeto `especialidade` |
| GET | `/usuarios` | Usuários sem o campo `senha` |
| GET | `/consultas` | Lista de consultas; lista vazia retorna `200` e `[]` |
| GET | `/consultas/:id` | Consulta pelo id ou `404` |
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

Para agilizar a conferência, importe `postman/clinica-api.postman_collection.json` no Postman. A coleção calcula as datas de teste no momento de cada envio; execute os requests na ordem apresentada. O primeiro GET `/consultas` retorna `[]` em um banco recém-criado; depois do POST válido, passa a listar a consulta criada.
