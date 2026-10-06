# Alhena FDE Take-Home: Keep the Catalog in Sync

Alhena builds AI shopping assistants for online stores. Our AI answers shoppers' questions about products, prices, and stock, right on the store's website.

As a Forward Deployed Engineer, you get stores live on Alhena. You connect their systems, make sure the AI has the right data, and work directly with their team.

This take-home is a small version of that job.

## The situation

Ridgeline Outfitters is an outdoor gear store that sells in the US and Canada. (It's made up for this exercise.) They go live with Alhena next week.

Our AI must always know the current price and stock of every product. If our copy of their catalog is out of date, the AI quotes wrong prices or recommends products that are gone.

Ridgeline built their own store platform. It has a REST API and sends webhooks. **Your job is to build the connector that keeps our copy of their catalog correct.**

Sam, Ridgeline's lead developer, sent you this:

> Hi! Excited to get Alhena live. Our API docs are in [docs/store-api.md](docs/store-api.md).
>
> A few tips:
> - Our webhooks are 100% reliable, so you don't need to poll. Please don't. Polling just eats your rate limit.
> - You get 10 requests per second.
> - All our timestamps are UTC.
> - Heads up: we run flash sales. Hundreds of prices can change at once.
>
> Shout if anything is unclear!
>
> Sam

## What you get

1. **The store.** A program that runs Ridgeline's store on your laptop. Download it from [Releases](https://github.com/nagendraK6/alhena-fde/releases).
2. **The API docs.** [docs/store-api.md](docs/store-api.md).
3. **Sam's note.** Above.

### Running the store

Download the file for your machine, then:

```bash
mv ridgeline-store-<your-platform> ridgeline-store
chmod +x ridgeline-store
xattr -d com.apple.quarantine ridgeline-store   # macOS only
./ridgeline-store
```

The store API runs at `http://localhost:7070`. The store sends webhooks to `http://localhost:8080/webhooks`.

Each run is a 10-minute business day:

- Prices and stock change all the time.
- Products are added, archived, and deleted.
- There's a flash sale.
- At minute 10 the store closes. Nothing changes after that, but the API stays up so you can check your work.

Every run starts a fresh day. The same seed always gives the same catalog and the same changes.

| Flag | Default | What it does |
|---|---|---|
| `--seed` | `1` | A different seed gives a different catalog and a different day. |
| `--minutes` | `10` | Length of the day. Use `3` for quick tests. |
| `--webhook-url` | `http://localhost:8080/webhooks` | Where the store sends webhooks. |
| `--port` | `7070` | Port for the store API. |

## What to build

A service that keeps an up-to-date copy of Ridgeline's catalog.

1. It runs on port `8080`.
2. It receives the store's webhooks at `POST /webhooks`.
3. It serves its copy of the catalog at `GET /catalog`, in the format below.
4. When something changes in the store, `/catalog` shows the change within 60 seconds.
5. `/catalog` lists only products shoppers can buy (status `active`). Archived and deleted products must not be in it.
6. It starts empty and learns everything from the store's API and webhooks.
7. It starts with one command on macOS or Linux.

Use any language, framework, and storage you like.

### `GET /catalog` format

```json
{
  "products": [
    {
      "id": 1042,
      "title": "Summit Dome Tent",
      "variants": [
        {
          "id": 50211,
          "sku": "SDT-2P-GRN",
          "title": "2-Person / Green",
          "price": { "USD": "249.00", "CAD": "339.00" },
          "compare_at_price": { "USD": null, "CAD": null },
          "available": 14
        }
      ]
    }
  ]
}
```

Values must match the store API exactly. Prices are strings. Order doesn't matter.

## What to send us

1. **Your code**, with a README that says how to start it in one command.
2. **NOTES.md**, two pages at most:
   - How your sync works.
   - Each place where the API doesn't behave the way the docs or Sam say, and how you proved it.
   - How you know your copy is correct.
   - What you'd do next with more time.
   - How you used AI, including one thing it got wrong.
3. **REPLY_TO_SAM.md**, your reply to Sam. 250 words at most.
4. **A video**, 5 minutes at most (Loom or similar). Walk us through what you built and why. Put the link in NOTES.md.

Share a private GitHub repo with [@nagendraK6](https://github.com/nagendraK6), or send a zip. Please don't post your solution publicly.

## How we'll look at it

1. We start your service, then the store, using a seed you haven't seen.
   - From minute 3 to minute 10, we check every few seconds that changes reach your `/catalog` within 60 seconds.
   - At minute 12, we compare your `/catalog` with the store. They should match exactly.
2. We read your notes and your reply to Sam, and watch your video.
3. Then we have a live call, about 45 minutes. You walk us through your work. Then we change something, and you adapt your code with us on the call.

## Ground rules

- **Time.** Spend about 5 hours. You have a week to send it. If you reach 5 hours, stop and write down what you'd do next. Good choices beat more code.
- **AI.** Use any AI tools you like. We use them every day. What we care about is your judgment: what you test, what you trust, and what you decide.
- **Questions.** Reply to the email that sent you this.
