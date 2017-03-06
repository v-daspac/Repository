# <a name="add-ins-for-outlook-mobile"></a>Suplementos do Outlook Mobile 

> **Observação:** há suplementos disponíveis no Outlook para iOS. O suporte do Outlook para Android está chegando.

Os suplementos agora funcionam no Outlook Mobile, usando as mesmas APIs disponíveis para outros pontos de extremidade do Outlook. Se você já tiver criado um suplemento para Outlook, é fácil fazê-lo funcionar no Outlook Mobile.

Os suplementos do Outlook Mobile são suportados em todas as contas do Office 365 Comercial e está sendo implantado nas contas do Outlook.com.

**Um painel de tarefas de exemplo no Outlook para iOS**

![Uma captura de tela do painel de tarefas no Outlook para iOS](../../images/outlook-mobile-addin-taskpane.png)

## <a name="whats-different-on-mobile"></a>Qual é a diferença no celular? 

- O tamanho pequeno e rápidas interações tornam o projeto para celular um desafio. Para garantir experiências de qualidade para nossos clientes, estamos definindo critérios rígidos de validação que devem ser cumpridos por um suplemento que declara suporte a celular de forma a ser aprovado na Office Store.
    - O suplemento **DEVE** cumprir as [diretrizes de interface do usuário](./outlook-addin-design.md).
    - O cenário do suplemento **DEVE** [fazer sentido no mobile](#what-makes-a-good-scenario-for-mobile-add-ins).
- Há suporte apenas para leitura de emails no momento. Isso significa que o `MobileMessageReadCommandSurface` é o único [ExtensionPoint](../../reference/manifest/extensionpoint.md) que você deve declarar na seção móvel de seu manifesto
- A API [makeEwsRequestAsync](../../reference/outlook/Office.context.mailbox.md) não é suportada no celular, já que o aplicativo móvel usa APIs REST para se comunicar com o servidor. Se seu back-end do aplicativo precisa se conectar ao servidor do Exchange, é possível usar o token de retorno de chamada para fazer chamadas de API REST. Para obter detalhes, consulte [Usar APIs REST do Outlook de um suplemento do Outlook](./use-rest-api.md).
- Quando você envia o suplemento para a loja com [MobileFormFactor](../../reference/manifest/mobileformfactor.md) no manifesto, precisará concordar com nosso adendo de suplementos no iOS e precisará enviar sua ID de desenvolvedor Apple para verificação.
- Por fim, seu manifesto precisará declarar `MobileFormFactor` e ter os tipos corretos de [controles](../../reference/manifest/control.md) e [tamanhos de ícone](../../reference/manifest/icon.md) incluídos.

## <a name="what-makes-a-good-scenario-for-mobile-add-ins"></a>O que forma um bom cenário para suplementos móveis?

Lembre-se de que o tamanho médio da sessão Outlook em um telefone é bem menor do que em um PC. Isso significa que seu suplemento deve ser rápido e o cenário deve permitir que o usuário entre, saia e prossiga com seu fluxo de email.

Estes são exemplos de cenários que fazem sentido no Outlook Mobile.

- O suplemento traz informações valiosas para o Outlook, para ajudar os usuários na triagem dos emails e a responder adequadamente. Exemplo: um suplemento CRM que permite ao usuário ver informações do cliente e compartilhar informações apropriadas.
- O suplemento agrega valor ao conteúdo do email do usuário, salvando as informações em um controle, uma colaboração ou um sistema semelhante. Exemplo: um suplemento que permite aos usuários ativar emails em itens de tarefa para acompanhamento de projetos, ou tíquetes de ajuda, para uma equipe de suporte.

Definitivamente, há outros ótimos cenários por aí, então se você tem uma ideia para um suplemento que vá além deles, entre em contato conosco usando o [formulário aqui](https://aka.ms/outlookmobileaddin) para receber comentários se esse é um cenário aceitável do Outlook Mobile. Ficaremos felizes de oferecer orientação e quanto mais informações você puder fornecer, melhor. Adoramos um bom passo a passo de interface do usuário!

**Uma interação de usuário de exemplo para criar um cartão do Trello com base em uma mensagem de email**

![Um GIF animado mostrando a interação do usuário com um suplemento do Outlook Mobile](../../images/outlook-mobile-addin-example.gif)

## <a name="testing-your-add-ins-on-mobile"></a>Teste seus suplementos no celular

Para testar um suplemento no Outlook Mobile, você pode carregar um suplemento para uma conta do Office 365 ou do Outlook.com. No Outlook Web App, acesse a engrenagem de configurações e escolha "Gerenciar integrações" ou "Gerenciar suplementos". Perto da parte superior, clique onde diz: "Clique aqui para adicionar um suplemento personalizado" e carregue seu manifesto. Verifique se seu manifesto está formatado corretamente para conter `MobileFormFactor` ou ele não será carregado.

Depois que seu suplemento estiver funcionando, certifique-se de testá-lo em tamanhos de tela diferentes, incluindo celulares e tablets. Você deve verificar se ele atende às diretrizes de acessibilidade de contraste, tamanho da fonte e cor, bem como de usabilidade com um leitor de tela, como o VoiceOver no iOS ou TalkBack no Android.

A solução de problemas no mobile pode ser difícil, já que você pode não ter as ferramentas com as quais está acostumado. Uma opção para solução de problemas é [usar Vorlon.js](../testing/debug-office-add-ins-on-ipad-and-mac.md). Ou, se tiver usado o Fiddler antes, confira [este tutorial sobre como usá-lo com um dispositivo iOS](http://www.telerik.com/blogs/using-fiddler-with-apple-ios-devices).

## <a name="next-steps"></a>Próximas etapas

- Saiba como [adicionar suporte móvel ao manifesto do seu suplemento](./manifests/add-mobile-support.md).
- Saiba como [projetar uma ótima experiência móvel para o seu suplemento](./outlook-addin-design.md).
- Saiba como [obter um token de acesso e chamar APIs REST do Outlook](./use-rest-api.md) do suplemento.