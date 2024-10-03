# Acesso ao Key Vault

Para ter acesso ao Key Vault, existem duas maneiras:

1. **Permissão para o usuário atribuindo no Key Vault**
2. **Identidades do espaço de trabalho**

## Opção 1: Permissão para o usuário

Basta atribuir a permissão no Key Vault:

![image](https://github.com/user-attachments/assets/10c05e71-9aa4-4620-8f9c-7fb3f7ac3173)


### No Notebook

```python
secret_value = mssparkutils.credentials.getSecret('https://uq-bi-key-vault.vault.azure.net/', 'BW')
print(secret_value)
print("Sucesso")
```

## Opção 2: Identidades do espaço de trabalho

Essa opção permite dar acesso ao workspace ao Key Vault, possibilitando a execução de pipelines sem necessariamente usar seu usuário. Como os pipelines são realizados por schedules, essa opção é a melhor, pois não há necessidade de usar sua credencial.

### Configuração do Workspace

1. Em Configuração de workspace, na opção **Identidade do espaço de Trabalho**, clique em **+ Identidade do espaço de trabalho**.

   ![image](https://github.com/user-attachments/assets/4394f1e6-a80d-45a2-ada3-04334b0d960f)


2. Automaticamente, o sistema criará uma identidade de espaço no Azure. É como se fosse uma identidade gerenciada do Azure, em que você pode atribuir permissões para aquela identidade. O nome da identidade é o nome do espaço de trabalho.

   O Fabrica usará identidades de espaço de trabalho para obter tokens do Microsoft Entra sem que o cliente precise gerenciar credenciais.

   Uma identidade de espaço de trabalho recebe automaticamente a função **Colaborador do espaço de trabalho** e tem acesso aos itens do espaço de trabalho.

   **Obs:** Somente os administradores do espaço de trabalho podem criar ou excluir a identidade gerenciada do espaço.

3. Criada a identidade, é possível atribuir controles sobre ela, no nosso caso, para o Key Vault.

### Atribuindo Permissões no Key Vault

1. No Key Vault, vá na opção **Add role assignment** e adicione a role.

  ![image](https://github.com/user-attachments/assets/6be55efd-7774-4215-a1a7-70b59a51cbc2)


2. Clique em **Next**.

3. Deixe a opção **User, group, or Service Principal** marcada e clique em **+ Select Members**.

   ![image](https://github.com/user-attachments/assets/f1dca222-ee6d-46b3-8275-609449a8707c)


4. Agora, aparece o aplicativo referente à identidade criada.

Com isso, já é possível ter acesso aos Secrets do Key Vault e, quando executar um pipeline, não ocorrerá problema de credenciais.
