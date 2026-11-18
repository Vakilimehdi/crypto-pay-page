# Crypto Pay — shareable payment link for freelancers

A single static page that lets a freelancer generate a shareable payment link and lets a client open it and pay in crypto. No server, no build step, no database, no account.

## What it is

- **Generator view (default):** the freelancer picks a chain (Solana or Ethereum), a token (SOL / USDC / USDT on Solana; ETH / USDT / USDC on Ethereum), enters their wallet address and an amount, optionally adds a label/memo, and gets a shareable payment link.
- **Pay view:** opening that link shows the client a clean "pay this freelancer" page — amount + token, the wallet address (truncated, with a copy button), and the label. On Solana it also offers a **Solana Pay deep link** (`solana:` URL) that opens the client's wallet with the payment pre-filled. On Ethereum it gives copy buttons for the address and amount, plus a short note (Ethereum has no reliable universal payment deep link).

## How to use

### As a freelancer
1. Open `index.html` (or host it anywhere static — GitHub Pages, Netlify Drop, any web host).
2. Pick your chain and token, paste your wallet address, enter the amount and an optional label.
3. Click **Generate payment link**, copy the link, and send it to your client (chat, email, invoice, QR code).

### As a client
1. Open the link the freelancer sent you.
2. Check the amount, token, and label. **Verify the wallet address with the freelancer through a channel you trust before you send anything.**
3. On Solana, tap **Open in wallet (Solana Pay)** or copy the address and amount into your wallet. On Ethereum, copy the address and amount into your wallet and send.

## How the shareable link works

The payment details are encoded in the URL hash, e.g.:

```
index.html#pay=<base64url JSON of {chain, token, address, amount, label}>
```

Everything lives in the link itself — there is no server storing anything, so the link works as long as the page is hosted. The page validates the details when it opens a pay link; a broken or tampered link falls back to the generator form with a warning.

Solana Pay links follow the Solana Pay spec: `solana:<address>?amount=<amount>&spl-token=<mint>&label=<label>` for SPL tokens (USDC / USDT), and a plain `solana:<address>?amount=<amount>` for SOL.

## Security notes

- **This page never holds funds or private keys.** It only displays an address and an amount; the actual payment happens in the payer's own wallet.
- **Always verify the address.** The payer should confirm the wallet address matches the freelancer through a trusted channel before sending. Crypto payments cannot be reversed.
- The link is not signed: anyone with the link can see the payment details. Never put sensitive information in the label/memo.
- The page validates addresses (Solana: 32–44 base58 characters; Ethereum: `0x` + 40 hex characters) and requires an amount greater than 0, but validation is not a substitute for checking the address yourself.

## Customization

Everything is in one file: edit the `TOKENS` object at the top of the `<script>` to change chains, tokens, or SPL token mints. Colors live in the `:root` CSS variables.

## License

MIT — see `LICENSE`.

---

## خلاصه فارسی

**صفحه پرداخت کریپتو برای فریلنسرها:** یک فایل HTML تنها، بدون سرور. فریلنسر آدرس کیف پول، زنجیره (سولانا یا اتریوم)، توکن و مبلغ را وارد می‌کند و یک لینک پرداخت قابل‌اشتراک می‌گیرد. مشتری با باز کردن لینک، مبلغ و آدرس را می‌بیند؛ در سولانا با Solana Pay مستقیم در کیف پول باز می‌شود و در اتریوم دکمه‌های کپی آدرس و مبلغ دارد. صفحه هرگز پول یا کلید خصوصی نگه نمی‌دارد؛ پیش از پرداخت، آدرس را حتماً با فریلنسر چک کنید.
