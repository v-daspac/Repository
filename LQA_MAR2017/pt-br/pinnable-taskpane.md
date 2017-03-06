# <a name="implement-a-pinnable-taskpane-in-outlook"></a>Implementar um painel de tarefas fixável no Outlook

A forma do [painel de tarefas](../add-in-commands-for-outlook.md#launching-a-task-pane) da experiência de usuário dos comandos do suplemento abre um painel de tarefas vertical à direita de um compromisso ou de uma mensagem aberta, permitindo ao suplemento fornecer a interface do usuário a fim de obter interações mais detalhadas (preenchimento de vários campos etc.). Esse painel de tarefas pode ser exibido no painel de leitura durante a exibição de uma lista de mensagens, permitindo o processamento rápido de uma mensagem.

No entanto, se o usuário abrir um painel de tarefas do suplemento em uma mensagem no painel de leitura e selecionar uma nova mensagem, o painel de tarefas será fechado automaticamente, por padrão. Para um suplemento bastante usado, o usuário pode optar por manter esse painel aberto, eliminando a necessidade de reativar o suplemento em cada mensagem. Com os painéis de tarefas fixáveis, o suplemento pode fornecer essa opção aos usuários.

> **Observação**: atualmente, os painéis de tarefas fixáveis são compatíveis apenas com o Outlook 2016, versão 7628.1000.

## <a name="support-taskpane-pinning"></a>Suporte para fixação do painel de tarefas

A primeira etapa consiste em adicionar o suporte para fixação no [manifesto](./manifests.md) do suplemento. Para fazer isso, adicione o elemento [SupportsPinning](../../../reference/manifest/action.md#supportspinning) ao elemento `Action`, que descreve o botão do painel de tarefas.

O elemento `SupportsPinning` é definido no esquema do VersionOverrides v1.1. Portanto, você deve incluir um elemento [VersionOverrides](../../../reference/manifest/versionoverrides.md) nas versões 1.0 e 1.1.

```xml
<!-- Task pane button -->
<Control xsi:type="Button" id="msgReadOpenPaneButton">
  <Label resid="paneReadButtonLabel" />
  <Supertip>
    <Title resid="paneReadSuperTipTitle" />
    <Description resid="paneReadSuperTipDescription" />
  </Supertip>
  <Icon>
    <bt:Image size="16" resid="green-icon-16" />
    <bt:Image size="32" resid="green-icon-32" />
    <bt:Image size="80" resid="green-icon-80" />
  </Icon>
  <Action xsi:type="ShowTaskpane">
    <SourceLocation resid="readTaskPaneUrl" />
    <SupportsPinning>true</SupportsPinning>
  </Action>
</Control>
```

Para ver um exemplo completo, confira o controle `msgReadOpenPaneButton` na [amostra de manifesto command-demo](https://github.com/jasonjoh/command-demo/blob/master/command-demo-manifest.xml).

## <a name="handling-ui-updates-based-on-currently-selected-message"></a>Atualizações de tratamento da interface do usuário com base na mensagem atualmente selecionada

Para atualizar a interface do usuário ou as variáveis internas do painel de tarefas com base no item atual, você deve registrar um manipulador de eventos para receber notificações das alterações.

### <a name="implement-the-event-handler"></a>Implementar o manipulador de eventos

O manipulador de eventos deve aceitar um parâmetro exclusivo, que é um objeto literal. A propriedade `type` desse objeto será definida como `Office.EventType.ItemChanged`. Ao chamar o evento, o objeto `Office.context.mailbox.item` já estará atualizado para refletir o item atualmente selecionado.

```js
function itemChanged(eventArgs) {
  // Update UI based on the new current item
  UpdateTaskPaneUI(Office.context.mailbox.item);
}
```

### <a name="register-the-event-handler"></a>Registrar o manipulador de eventos

Use o método [Office.context.mailbox.addHandlerAsync](https://dev.outlook.com/reference/add-ins/1.5/Office.context.mailbox.html#addHandlerAsync) para registrar o manipulador de eventos para o evento `Office.EventType.ItemChanged`. Você deve fazer isso na função `Office.initialize` do painel de tarefas.

```js
Office.initialize = function (reason) {
  $(document).ready(function () {

    // Set up ItemChanged event
    Office.context.mailbox.addHandlerAsync(Office.EventType.ItemChanged, itemChanged);

    UpdateTaskPaneUI(Office.context.mailbox.item);
  });
};
```

## <a name="additional-resources"></a>Recursos adicionais

Para um exemplo de suplemento que implementa um painel de tarefas fixável, confira [command-demo](https://github.com/jasonjoh/command-demo) no GitHub.