# Alhena FDE Take-Home: Shop From Inside Claude or ChatGPT

Alhena builds AI shopping assistants for online stores. As a Forward Deployed Engineer, you get stores live on Alhena. You connect their systems to AI, make it work well, and keep it safe.

This take-home is a small version of that job.

## The situation

More and more shoppers start in ChatGPT and Claude. A store we work with wants shoppers to find products, build a cart, and check out without leaving the chat.

The store runs on Shopify. For this exercise, use Shopify's demo store, **mock.shop**:

- API: `https://mock.shop/api` (Shopify Storefront API, GraphQL). No account or key needed.
- See the store and try queries: [mock.shop](https://mock.shop)

**Your job is to build an MCP server for this store, connect it to Claude or ChatGPT, and make shopping in the chat work well, and safely.**

## What to build

1. An MCP server on top of the mock.shop API.
2. You decide which tools it has. Design them for an AI to use, not as a copy of the API.
3. Put it on the internet (HTTPS) so Claude or ChatGPT can reach it. Any host is fine.
4. Connect it to Claude (custom connector) or ChatGPT (developer mode) and use it there.

In a normal chat, a shopper should be able to:

- find products by describing what they want ("something warm for winter under $100"),
- see details, and which sizes and colors are in stock,
- compare a few options,
- build and change a cart,
- get a checkout link,
- ask about returns and shipping.

Use any language and any MCP library you like.

## What to send us

1. **Your code**, with a README that says how to run and deploy it.
2. **The URL of your MCP server.** Keep it running for 2 weeks after you send it.
3. **NOTES.md**, two pages at most:
   - Your tools: what each one does, and why you designed it that way. What you tried that didn't work.
   - Where the API surprised you, and what you did about it.
   - Security: what could go wrong when an AI can act on a store, and what you did about it.
   - What you'd do next with more time.
   - How you used AI, including one thing it got wrong.
4. **A video**, 5 minutes at most (Loom or similar). Show a real shopping chat in Claude or ChatGPT using your server. Then walk us through your choices. Put the link in NOTES.md.

Share a private GitHub repo with [@nagendraK6](https://github.com/nagendraK6), or send a zip. Please don't post your solution publicly.

## How we'll look at it

1. We connect your server to Claude or ChatGPT and shop with it. We also try to break it.
2. We read your notes and watch your video.
3. Then we have a live call, about 45 minutes. You walk us through your work, and we talk about security. Then we change something, and you adapt your server with us on the call.

## Ground rules

- **Time.** Spend about 5 hours. You have a week to send it. If you reach 5 hours, stop and write down what you'd do next. Good choices beat more code.
- **AI.** Use any AI tools you like. We use them every day. What we care about is your judgment: what you test, what you trust, and what you decide.
- **Questions.** Reply to the email that sent you this.
