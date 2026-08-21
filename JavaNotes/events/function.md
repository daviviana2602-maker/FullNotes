================= RabbitMQ — Qual problema resolve? =================


**Exemplo prático (pix)**


PIX de 1500 reais:
██████████ ✓ concluído em 100ms


Email ou SMS lento (é realizado pós pix acima de 1000 reais):
████████████████████████████████ 10 minutos



Sem assíncrono:

   PIX (JÁ ACONTECEU)
         ↓
   espera email
         ↓
      espera SMS
         ↓
   só apos TUDO, responde usuário


Com RabbitMQ:

                              PIX (JÁ ACONTECEU)
                                     ↓
                              publica eventos
                                     ↓
   já responde usuário e deixa serviços secundários processarem no seu próprio tempo
                        (processam quando ouvem o evento)


Enquanto isso:

   RabbitMQ
   ├──→ Consumer Email
   └──→ Consumer SMS


- CONCLUSÃO:

   O PIX já aconteceu, então não faz sentido prender a resposta ao usuário por SERVIÇOS SECUNDÁRIOS que podem demorar ou estar indisponíveis.