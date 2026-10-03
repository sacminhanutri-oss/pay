# Checkout TikTok Pay + Mangofy + RePix AI

Pasta contendo exclusivamente os arquivos necessários para colocar seu checkout em produção. Todos os estilos CSS, imagens (banner e produto em base64) e bibliotecas de ícones SVG já estão 100% integrados em um arquivo autossuficiente.

---

## Arquivos Disponíveis

| Arquivo | Descrição |
|---|---|
| `index.php` | Arquivo principal pronto para hospedagem PHP (cPanel, Hostinger, VPS, etc.). |
| `checkout.php` | Cópia idêntica de `index.php` caso sua rota utilize `/checkout.php`. |
| `index.html` | Versão estática equivalente (para visualização rápida no navegador). |

---

## 1. Onde Configurar suas Chaves

Abra o arquivo `index.php` em qualquer editor de código. Logo nas primeiras linhas (topo do arquivo), você encontrará o bloco de configuração:

```php
<?php
// =============================================================================
// CONFIGURAÇÕES GERAIS - PREENCHA SUAS CHAVES, PIXEL E URL DE UPSELL AQUI
// =============================================================================

$MANGOFY_API_KEY    = ""; // Sua API Key da Mangofy (Header: Authorization)
$MANGOFY_STORE_CODE = ""; // Seu Store Code da Mangofy (Header: Store-Code)
$TIKTOK_PIXEL_ID    = ""; // Seu Pixel do TikTok (Ex: D8GOVLBC77UANKFSAPKG)
$UPSELL_URL         = ""; // URL de Upsell após confirmação do pagamento (ex: /upsell-1)
$MANGOFY_BASE_URL   = "https://api.mangofy.com.br"; // Endpoint base da Mangofy
$REPIX_API_URL      = "https://api.repix.site";     // API de Auditoria RePix

// =============================================================================
// FIM DAS CONFIGURAÇÕES
// =============================================================================
?>
```

---

## 2. Fluxo de Funcionamento

1. **Entrada do Cliente:** O comprador informa apenas o **Nome Completo**.
2. **Payload para Mangofy:**
   - `doc`: fixo `11144477735`
   - `phone`: fixo `12345678910`
   - `email`: gerado automaticamente no formato `nome.sobrenome@dominio.com`
3. **Geração do Pix:** Exibe o QR Code e o código Pix Copia e Cola instantaneamente.
4. **Verificar Pagamento (RePix):**
   - Ao clicar em **"Verificar Pagamento"**, o sistema consulta `https://api.repix.site/api/v1/checkout/status/{sale_id}`.
   - **Se confirmado (`is_paid === true`):** O comprador é redirecionado imediatamente para a URL de Upsell sem nenhum atrito.
   - **Se pendente:** Abre o Pop-up Modal discreto (sem bolinhas e sem emojis) para envio do comprovante Pix auditado por Inteligência Artificial sob demanda.
5. **Pixel do TikTok:**
   - `InitiateCheckout`: disparado no carregamento da página.
   - `Purchase`: disparado na aprovação do pagamento ou validação do comprovante.