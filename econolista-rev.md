# Aplicativo crossplatform com Expo: uma análise técnica detalhada


## Uma analise tècnica do que deu certo e o que não deu no desenvolvimento de um aplicativo crossplatform com Expo

---

## O problema

Em 2025 decidi retomar o desenvolvimento de um aplicativo de lista de compras que eu tinha feito para minha mulher, e que infelizmente foi descontinuiado da loja, não me atentei as atualizaçóes de uso da plataforma e ele acabou sendo removido. O codigo fonte estava no meu Github, mas se eu fosse recomeçar, queria dessa vez ter controle dos dados, jà que a primeira versáo era offiline first, feito com RealmDB(MongoDB) mas sem um banco de dados externo(online), logo meus dados e os dos mais de 200 usuários foram perdidos, sem possibilidade de recuperação depois que o app era desinstalado.

Como eu estava sem uma conta da PlayStore, mas náo queria ficar sem meu aplicativo, ou ter retrabalhos no futuro, decidi recomeçar o projeto, dessa vez com um aplicativo **crossplatform** que eu pudesse reaproveitar o còdigo fonte, em diversos dispositivos, mantendo o minimo de trabalho no futuro quando eu fosse ajustar par a loja.

Decidi manter o **Expo** como base para o desenvolvimento, jà que eu poderia reaproveitar grande parte da lògica que eu já havia feito na primeira versáo, e que eu tinha feito com o **React Native**, e seria uma boa oportunidade para testar o entáo a pouco tempo lançado **expo router**, mesmoo sabendo o tamanho do desafio decidi me aventurar.


## Em busca de um boilerplate-
https://thecodingmachine.github.io/
react-native-boilerplate/

https://infinite.red/ignite

https://rn.new/
