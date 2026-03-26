# Avaliação — Engenharia de Software
**Sistema Integrado de Gestão de Farmácia — MVP Definido pelo Estudante**

Aluno: Amanda Rodrigues Pristupa Martins
RA: 25000009  
Data: 25/03/2026  

---

# 1. Definição do MVP
O MVP deste sistema foca em um sistema de gestão de farmácia, com o objetivo de controlar os processos de vendas, estoques, compras, clientes, a parte financeira (tanto o que se paga, quanto o que se recebe) e os indicadores de desempenho.

- **Dentro** do MVP temos os processos de vendas, o cadastro de pordutos e de clientes, atualização dos estoques, os processos de contas a pagar e a receber e os registros do caixa.

- **Fora** do MVP temos a gestão de compras/pedidos, a gestão de pagamentos e transferência de unidades de produtos.  

Os problemas de divergência de estoque entre unidades, falhas no registro de vendas e lançamentos financeiros são as principais dores relatadas pela diretoria, e para solucionar este problema, foi autorizado o desenvolvimento do sistema de gestão para a farmácia.

---

# 2. Regras de Negócio 
**RN01 —** Vendas controladas: os medicamentos controlados precisam de uma receita que seja validada pelo farmacêutico.

**RN02 —** Insuficiência de estoque: o sistema só pode autorizar uma venda caso a quantidade pedida pelo cliente esteja dentro do estoque.

**RN03 —** Vendas a prazo: apenas clientes "fiéis" e que estão em dia com seus pagamentos na farmácia podem escolher realizar uma compra "a prazo".

**RN04 —** Alertas para situações críticas: o sistema tem que obrigatoriamente avisar o gerente quando algum pedido ultrapassar
 a quanidade mínima/máxima de produtos que está no cadastro de um cliente.

**RN05 —** Atualização automática do estoque: após cada venda, os itens do estoque correspondentes a venda devem ser atualizados em tempo real.


---

# 3. Requisitos Funcionais 
**RF01 —** O sistema tem que permitir o cadastro e a consulta de produtos (nome, código, preço).

**RF02 —** O sistema tem que registrar todas as vendas (a vista e a prazo) 

**RF03 —** O sistema tem que permitir que os clientes se cadastrem no momento da compra.

**RF04 —** O sistema tem que emitir um comprovante de venda (recibo).

**RF05 —** O sistema também tem que permitir os usuários da farmácia (gerentes e farmacêuticos) para que possam modificar a quantidade que está no estoque.

**RF06 —** O sistema tem que gerar um lançamento de "contas a receber" para o gerente após grandes vendas.

**RF07 —** O sistema tem que permitir que os usuários da farmácia deixem uma receita médica retida.

**RF08 —** O sistema deve gerar relatórios dos produtos que são mais vendidos para estar sempre com a reposição nos estoques.

---

# 4. Requisitos Não Funcionais 
**RNF01 —** Disponibilidade: o sistema deve sempre estar disponível em horário comercial.

**RNF02 —** Performance: a consulta de produtos por código deve ser rápida.

**RNF03 —** Segurança: O sistema tem que controlar o acesso dos perfis.

**RNF04 —** Integridade: O sistema deve estar sempre atento quanto a modificação nos estoques caso ocorra uma falha na venda.

---

# 5. Casos de Uso
### **Diagrama de casos de uso geral**:
<img width="1344" height="839" alt="image" src="https://github.com/user-attachments/assets/b5760999-5199-4307-a9f4-fdf0785046ec" />

UC01 - Efetuar Venda (Atendente)

UC02 - Consultar Estoque (Atendente/Gerente) -> <<include>> em UC01.

UC03 - Cadastrar Cliente (Atendente) -> <<extend>> em UC01 (se cliente não cadastrado).

UC04 - Autorizar Medicamento Controlado (Farmacêutico) -> <<extend>> em UC01.

UC05 - Registrar Conta a Receber (Sistema) -> <<include>> em UC01 (se venda a prazo).

UC06 - Emitir Comprovante (Sistema) -> <<include>> em UC01.

UC07 - Atualizar Estoque Manualmente (Gerente)

UC08 - Gerar Relatório de Vendas (Gerente/Financeiro)

UC09 - Manter Cadastro de Produto (Gerente)

UC10 - Aplicar Desconto Especial (Gerente) -> <<extend>> em UC01.


---

# 6. Documentação dos Casos de Uso
## **UC01 — Efetuar Venda**
**Ator(es):** Atendente
**Descrição:** Registro da venda dos produtos de um cliente no balcão.
**Pré-condições:** Usuário autenticado e produtos com estoque disponível.
**Pós-condições:** Estoque atualizado, comprovante emitido e registro financeiro gerado.

### Fluxo Principal
1. O atendente inicia a venda.
2. O atendente informa o código do produto.
3. Sistema confere o estoque.
4. Sistema adiciona item ao carrinho e exibe o valor.
5. O atendente finaliza a venda e escolhe a forma de pagamento.
6. Sistema gera o comprovante da venda.

### Fluxos Alternativos / Exceções
- FA01 — Cliente Novo: Se o cliente não possuir cadastro, o atendente aciona o cadastro rápido.
- FA02 — Produto Controlado: Se o produto pedir uma receita, o sistema solicita a validação do Farmacêutico.
- FA03 - Venda a Prazo: Se o pagamento for a prazo, o sistema tem que registrar como conta a receber.

### Relacionamentos
- **Include:** UC02, UC06.
- **Extend:**  UC03, UC04, UC05. 

<img width="817" height="806" alt="image" src="https://github.com/user-attachments/assets/6270c46c-f49a-44c2-a387-ec1994cab73a" />

-
## **UC02 — Consultar Estoque**
**Ator(es):** Atendente, Gerente
**Descrição:**  Permite verificar a quantidade disponível e o preço de um produto no estoque.
**Pré-condições:** Usuário autenticado.
**Pós-condições:** Informações do produto exibidas na tela.

### Fluxo Principal
1.O usuário acessa a tela de consulta dos produtos.
2.O usuário informa o nome ou código do produto.
3.O sistema busca as informações no banco de dados.
4.O sistema exibe a descrição, preço e quantidade em estoque.


### Fluxos Alternativos / Exceções
- FA01 — Produto Não Encontrado: Se o código informado não existir, o sistema deve exibir uma mensagem de erro e solicitar uma nova digitação.

### Relacionamentos
- **Include:**(Nenhum)
- **Extend:**  (Nenhum)

<img width="405" height="777" alt="image" src="https://github.com/user-attachments/assets/195f3318-ffc5-4a0e-a7ae-ca2ec5c8cba7" />

-
## **UC03 — Cadastrar Cliente**
**Ator(es):** Atendente
**Descrição:** Realiza o cadastro de um novo cliente no sistema para permitir venda a prazo.
**Pré-condições:** Usuário autenticado.
**Pós-condições:** Cliente cadastrado no banco de dados.

### Fluxo Principal
1.O atendente acessa a tela de cadastro de cliente.
2.O atendente solicita e insere o CPF, Nome e Telefone do cliente.
3.O sistema valida os dados e verifica se o CPF já existe.
4.O sistema salva o novo cliente.

### Fluxos Alternativos / Exceções
-FA01 — CPF Já Cadastrado: Se o CPF já existir no banco, o sistema avisa e carrega os dados existentes, encerrando o cadastro de um novo cliente.

### Relacionamentos
- **Include:**(Nenhum)
- **Extend:**  (Nenhum)
  
<img width="439" height="904" alt="image" src="https://github.com/user-attachments/assets/31d62fb5-3e5c-4a93-ad77-aaf64a05dc7f" />

-

## **UC04 — Autorizar Medicamento Controlado**
**Ator(es):** Farmacêutico
**Descrição:** O farmacêutico valida a receita médica e autoriza a venda de um medicamento restrito.
**Pré-condições:**  Venda em andamento contendo o item controlado.
**Pós-condições:** Item liberado para venda e dados da receita registrados.

### Fluxo Principal
1.O sistema solicita autenticação do Farmacêutico na hora da venda.
2.O Farmacêutico insere suas credenciais.
3.O Farmacêutico confere a receita e digita os dados (Nome Médico, CRM, Data...).
4.O sistema valida os dados e libera o item.

### Fluxos Alternativos / Exceções
-FA01 — Receita Inválida: Se a receita não estiver em conformidade, o Farmacêutico recusa no sistema, e o item é removido da venda.

### Relacionamentos
- **Include:**(Nenhum)
- **Extend:**  (Nenhum)

<img width="445" height="904" alt="image" src="https://github.com/user-attachments/assets/5f8b6f87-2b2d-47e6-8fa0-cd33baa48171" />

-

## **UC05 — Registrar Conta a Receber**
**Ator(es):** Sistema
**Descrição:** Cria automaticamente um lançamento financeiro de débito para o cliente quando uma venda é finalizada em "A Prazo".
**Pré-condições:** Venda finalizada como "A Prazo".
**Pós-condições:** Lançamento criado no financeiro vinculado ao cliente.

### Fluxo Principal
1.O sistema recebe os dados da venda finalizada (Cliente, Valor Total, Data).
2.O sistema calcula a data de vencimento.
3.O sistema cria um registro na tabela de "Contas a Receber" com status de "Aberta".

### Fluxos Alternativos / Exceções
-FA01 — Falha no Registro: Em caso de erro ao salvar, o sistema deve desfazer a finalização da venda.

### Relacionamentos
- **Include:**(Nenhum)
- **Extend:**  (Nenhum)

<img width="468" height="777" alt="image" src="https://github.com/user-attachments/assets/a15e5a32-e5a7-481f-8872-787cc37fb042" />

-

## **UC06 — Emitir Comprovante**
**Ator(es):** Sistema
**Descrição:** Gera e envia para impressão o recibo da venda contendo os itens, valores e dados da farmácia.
**Pré-condições:** Venda paga com sucesso.
**Pós-condições:** Impressão enviada à impressora.

### Fluxo Principal
1.O sistema compila os dados da venda.
2.O sistema formata o layout do recibo.
3.O sistema envia o comando para a impressora.

### Fluxos Alternativos / Exceções
-FA01 — Impressora Off-line: Se a impressora não responder, o sistema exibe mensagem de erro e oferece opção de reimprimir depois.

### Relacionamentos
- **Include:**(Nenhum)
- **Extend:**  (Nenhum)

<img width="418" height="904" alt="image" src="https://github.com/user-attachments/assets/518549f8-5d2d-451a-bb68-dd1b8d9737f8" />

-

## **UC07 —  Atualizar Estoque Manualmente**
**Ator(es):** Gerente
**Descrição:** Permite que o gerente ajuste a quantidade de um produto no estoque para corrigir divergências encontradas.
**Pré-condições:** Usuário autenticado como Gerente.
**Pós-condições:** Quantidade de estoque alterada e log de auditoria registrado.

### Fluxo Principal
1.O gerente busca o produto desejado.
2.O gerente seleciona a opção "Ajuste de Estoque".
3.O gerente informa a nova quantidade real.
4.O gerente insere uma justificativa.
5.O sistema atualiza o saldo e grava o log de quem alterou e porquê.

### Fluxos Alternativos / Exceções
-FA01 — Justificativa Vazia: O sistema impede o salvamento caso o campo de justificativa esteja vazio.

### Relacionamentos
- **Include:**(Nenhum)
- **Extend:**  (Nenhum)
  
<img width="716" height="595" alt="image" src="https://github.com/user-attachments/assets/98c1adb9-04a4-465a-9cfa-20e544075702" />

-

## **UC08 — Gerar Relatório de Vendas**
**Ator(es):**  Gerente, Financeiro
**Descrição:** Gera um relatório consolidado das vendas realizadas em um determinado período para análise de desempenho e fechamento de caixa.
**Pré-condições:** Usuário autenticado com perfil permitido.
**Pós-condições:** Relatório exibido na tela.

### Fluxo Principal
1.O usuário acessa o módulo de relatórios.
2.O usuário seleciona "Vendas por Período".
3.O usuário informa a data inicial e data final.
4.O sistema processa os dados e exibe o total vendido, total à vista e total a prazo.

### Fluxos Alternativos / Exceções
-FA01 — Período Inválido: Se a data final for menor que a inicial, o sistema exibe erro.
-FA02 — Sem Dados: Se não houver vendas no período, o sistema avisa "Nenhuma venda encontrada".

### Relacionamentos
- **Include:**(Nenhum)
- **Extend:**  (Nenhum)
- 
<img width="815" height="694" alt="image" src="https://github.com/user-attachments/assets/0c35878d-35f2-47af-bcdd-0b8cf41a4a67" />


-

## **UC09 — Manter Cadastro de Produto**
**Ator(es):** Gerente
**Descrição:** Permite cadastrar novos produtos ou alterar informações de produtos existentes.
**Pré-condições:** Usuário autenticado como Gerente.
**Pós-condições:** Base de produtos atualizada.

### Fluxo Principal
1.O gerente acessa a tela de cadastro de produtos.
2.O gerente escolhe "Novo" ou "Editar" um existente.
3.O gerente preenche/altera os dados.
4.O sistema valida os campos obrigatórios e salva as alterações.

### Fluxos Alternativos / Exceções
-FA01 — Preço Inválido: Se o preço for zero ou negativo, o sistema impede o salvamento.

### Relacionamentos
- **Include:**(Nenhum)
- **Extend:**  (Nenhum)
  
<img width="713" height="595" alt="image" src="https://github.com/user-attachments/assets/2092de5a-a518-4b89-986a-ad7c4e8239cd" />

-

## **UC10 — Aplicar Desconto Especial**
**Ator(es):** Gerente
**Descrição:** Permite que o gerente autorize um desconto superior ao padrão permitido ao atendente em uma venda específica.
**Pré-condições:** Venda em andamento.
**Pós-condições:** Valor total da venda recalculado com o novo desconto.

### Fluxo Principal
1.Na tela de venda, o atendente solicita "Desconto Gerencial".
2.O sistema solicita autenticação do Gerente.
3.O Gerente insere credenciais e o valor do desconto.
4.O sistema aplica o desconto sobre o total do carrinho.


### Fluxos Alternativos / Exceções
-FA01 — Gerente Não Autenticado: Se o gerente falhar na senha, o desconto não é aplicado.

### Relacionamentos
- **Include:**(Nenhum)
- **Extend:**  (Nenhum)
- 
<img width="463" height="777" alt="image" src="https://github.com/user-attachments/assets/821cb17b-d300-43ff-9e94-1ab9d9082bd7" />

---



