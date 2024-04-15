
# Projeto de compra de ingresso Rock in Rio

Embora a imagem do projeto no repositório represente o fluxo da aplicação, fornecendo uma visão geral de como ela deve funcionar em produção, este README destina-se a fornecer insights mais detalhados sobre aspectos técnicos que podem ser implementados para aprimorar a experiência do usuário. Vamos explorar algumas práticas e considerações importantes para garantir que a aplicação opere de maneira eficiente e ofereça um alto nível de satisfação aos usuários finais



## Load balance
O Load Balancer desempenha um papel importante em nosso projeto, distribuindo o tráfego de entrada entre vários servidores web. Essa distribuição equitativa de solicitações ajuda a evitar sobrecargas em qualquer servidor específico, garantindo assim um desempenho estável e confiável da aplicação. Além disso, durante picos de tráfego, o Load Balancer assegura que o site permaneça acessível ao distribuir o fluxo de solicitações de forma eficiente entre os servidores disponíveis, melhorando a escalabilidade e a capacidade de resposta do sistema.

## Sistema de filas
É fundamental implementar um sistema de fila que gerencie a entrada dos usuários no portal para a seleção de ingressos, garantindo que a ordem de chegada seja respeitada. Esse sistema não apenas reduz a ansiedade dos usuários, fornecendo informações sobre sua posição na fila, mas também contribui para a estabilidade do servidor, minimizando a quantidade de atualizações na página e evitando possíveis sobrecargas. É importante destacar que o sistema de filas deve ser robusto e resistente a interrupções, permanecendo inalterado mesmo em caso de atualizações ou falhas na conexão. Ou seja, uma vez que um usuário entre na fila, sua posição deve ser preservada até que ele acesse o portal. Além disso, a tela deve ser atualizada automaticamente em intervalos regulares, fornecendo aos usuários uma visão em tempo real da situação da fila.

## Middleware verifica estoque
O middleware 'verifica estoque' desempenha um papel crucial em dois pontos-chave do nosso sistema: primeiro, ao acessar o portal pela primeira vez e segundo, ao selecionar e confirmar os ingressos para prosseguir para a tela de pagamento. Essa abordagem estratégica nos permite controlar rigorosamente o processo de emissão de ingressos com base em sua disponibilidade em tempo real. Ao integrar esse middleware em etapas estratégicas do processo de compra, garantimos uma experiência fluida para os usuários, evitando vendas de ingressos além da capacidade disponível e reduzindo o risco de sobrecarga do sistema.

## Sessão
Assim que chega a vez do usuário e houver ingressos disponíveis, uma sessão deve ser aberta para ele, com um tempo limite de 15 minutos. Esse intervalo garante que o ingresso não seja retido indefinidamente, permitindo que outros usuários tenham a oportunidade de adquiri-lo. Além disso, para otimizar o desempenho e garantir uma experiência rápida e responsiva, utilizamos o cache para armazenar os dados da sessão. Dessa forma, evitamos consultas frequentes ao banco de dados e garantimos que as informações da sessão permaneçam acessíveis durante o tempo limite estabelecido. Podemos renover o tempo da sessão do usuário ao acessar a etapa de pagamento, exibindo o cronometro do tempo que ele tem para efetuar o pagamento.

## Reserva de ingresso
Após serem selecionados, confirmados e validados, os ingressos devem ser reservados exclusivamente para aquela sessão e não devem mais ser contabilizados no estoque disponível. Os ingressos somente serão liberados em caso de cancelamento do pagamento ou se o tempo limite da sessão expirar. Essa abordagem garante que os usuários tenham uma janela de oportunidade justa para concluir o processo de compra sem o risco de perderem os ingressos devido a indisponibilidade

## Confirmação do pagamento
Após a confirmação do pagamento, os ingressos serão automaticamente atribuídos ao usuário logado e não estarão mais disponíveis para comercialização em nenhuma circunstância.
