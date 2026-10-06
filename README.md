# Alhena FDE Take-Home: Shop From Inside Claude or ChatGPT

Alhena builds AI shopping assistants for online stores. As a Forward Deployed Engineer, you get stores live on Alhena. You connect their systems to AI, make it work well, and keep it safe.

This take-home is a small version of that job.

## The situation

More and more shoppers start in ChatGPT and Claude. A store we work with wants shoppers to find products, build a cart, and check out without leaving the chat.

The store runs on Shopify. For this exercise, use Shopify's demo store, **mock.shop**.

**Your job is to build an MCP server for this store, connect it to Claude or ChatGPT, and make shopping in the chat work well, and safely.**

## The store API

- Address: `https://mock.shop/api`. It's the Shopify Storefront API (GraphQL).
- No account, key, or token needed.
- Try it:

  ```bash
  curl https://mock.shop/api -H 'Content-Type: application/json' \
    -d '{"query":"{ products(first: 3) { edges { node { title } } } }"}'
  ```

- Try queries in your browser at [mock.shop](https://mock.shop).
- Reference: [Shopify Storefront API docs](https://shopify.dev/docs/api/storefront).

## What your MCP server should do

### What shoppers can do

1. **Find products** by describing what they want, like "something warm for winter under $100". Results should match what the shopper meant, not just the words they typed. Shoppers can narrow by price, category, size, color, and stock.
2. **See product details:** description, price, every size and color, and whether each one is in stock.
3. **Compare** two to four products side by side.
4. **Get suggestions** for similar or matching items.
5. **Manage a cart:** add a product in a specific size and color, change quantities, remove items, see what's in the cart and the total, and try a discount code.
6. **Check out:** get the checkout link. The AI never handles payment.
7. **Ask about store policies:** returns, shipping, and privacy, answered from the store's own policy text.

You decide which tools give shoppers these abilities.

### How it should behave

- The tools are made for an AI: clear names and descriptions, simple inputs, and short, useful results. Not raw GraphQL.
- The shopper always knows exactly which size and color they're getting.
- Prices always show the currency.
- When something fails, the AI gets a clear message it can act on, like "Medium is out of stock. Small and Large are available."
- The AI never says something worked when it didn't.
- The cart changes only when the shopper asks.
- A shopper's cart stays with them during the chat. One shopper must never see or change another shopper's cart.
- Product text from the store is treated as information, never as instructions.

### Technical requirements

- It's a remote MCP server over HTTPS, so Claude or ChatGPT can connect to it.
- It works in Claude (custom connector) or ChatGPT (developer mode). One is enough.
- It runs locally with one command.
- It handles bad input and heavy traffic without falling over.
- No secrets in the code.
- A few tests for the parts that matter most.

Use any language, host, and MCP library you like.

### A chat it should handle

1. "I need something warm for winter, under $100."
2. "Show me the hoodie in green. Is medium in stock?"
3. "Compare the Puffer and the Light Puffer."
4. "Add a medium green hoodie and the Puffer in large to my cart."
5. "Actually, make it two hoodies, and remove the Puffer."
6. "Apply the code SAVE10."
7. "What's your return policy?"
8. "OK, I'm ready to check out."

We'll also try chats you haven't seen.

### If you have time (optional)

- Show products as cards with images in the chat, for example with OpenAI's Apps SDK or MCP Apps.

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
