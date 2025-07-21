Objetivo
Capturar todos os campos (visíveis e ocultos) do formulário do Elementor, incluindo UTMs, e enviá‑los a um webhook antes que a página redirecione ou recarregue – sem quebrar o comportamento padrão do plugin.

📂 Estrutura do script
html
Copiar
Editar
<script>
/* 1. Espera o DOM carregar */         document.addEventListener('DOMContentLoaded', …
/* 2. Localiza o form Elementor */      var form = document.querySelector('form.elementor-form');
/* 3. Intercepta o submit */            form.addEventListener('submit', function(event) { … });
/* 4. Serializa todos os campos */      var formData = new FormData(form);
/* 5. Monta objeto JSON */              formData.forEach((value,key)=>{ data[key]=value });
/* 6. Define URL do webhook */          var webhookURL = 'https://seu-webhook-url.com/endpoint';
/* 7. Envia de forma assíncrona */      navigator.sendBeacon(...)  ||  fetch(... { keepalive:true });
</script>
Por que navigator.sendBeacon?
Garantia de envio: executa mesmo se o navegador fizer redirect ou fechar guia.

Não bloqueia o thread principal; zero impacto na UX.

fetch(..., { keepalive:true }) entra como fallback para browsers antigos.

🚀 Passo‑a‑passo de uso
Cole o script

Vá em Elementor → Configurações → Custom Code (ou outro local de scripts).

Insira o bloco antes do </body> para pegar o DOM pronto.

Edite a URL do webhook

js
Copiar
Editar
var webhookURL = 'https://api.meu‑crm.com/webhooks/lead';
Ajuste o seletor do formulário (opcional)
Se seu form tiver ID/Classe específica, melhore a precisão:

js
Copiar
Editar
var form = document.querySelector('#form‑lead‑principal');
Publique e teste

Abra a DevTools → Network → filtre por collect ou pelo domínio do webhook.

Submeta o form; confirme o Status 200.

🛠️ Como funciona – por baixo dos panos
Etapa	O que acontece	Benefício
DOMContentLoaded	Garante que o script veja o form renderizado	Evita null em SPA Elementor
FormData API	Lê todos inputs (<input>, <select>, <textarea>)	Inclui UTMs escondidas
Objeto data	Serializa pares nome → valor	Fica pronto para JSON
sendBeacon	Cria um blob e envia POST assíncrono	Sobrevive a page unload
Fallback fetch	Usa keepalive:true	Compatibilidade total

💡 Customizações rápidas
Quer fazer…	Troque / adicione
Filtrar campos	Antes de montar payload, delete chaves indesejadas (delete data.unwanted)
Campos calculados	data.timestamp = new Date().toISOString()
Webhook autenticado	1) Mude webhookURL += '?token=XYZ' ou 2) adicione cabeçalho Authorization no fetch fallback
Vários formulários	Use querySelectorAll e faça forms.forEach(bindListener)

🔍 Debug & troubleshooting
Sintoma	Possível causa	Solução
Console: “Formulário não encontrado”	Seletor incorreto	Ajuste querySelector
Webhook recebe vazio	Campos ocultos não existem no DOM	Confirme que as UTMs já foram injetadas antes do submit
Erro CORS	Webhook não permite origem	Configure headers Access-Control-Allow-Origin no servidor
Firefox antigo não envia	Falta sendBeacon + fetch keepalive	O fallback já cobre; teste em > v57

📜 Licença
MIT – use, modifique e compartilhe à vontade. Só não esqueça de brindar ao sucesso das suas campanhas. 🥂

🚀 Palavra final
Este snippet é plug‑and‑play: cole, ajuste a URL e boas conversões!
Qualquer dúvida ou melhoria, abra uma issue ou chame aqui. 😉

“Dados valem ouro. Webhook certo é o garimpo sem pepino.”
