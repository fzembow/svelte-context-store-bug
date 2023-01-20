# How was this repro created

First, I installed svelte kit

```
npm create svelte@latest store-bug
```

Then, I edited [`src/routes/+page.svelte`](/src/routes/+page.svelte) to set a context containing a store.

Lastly, I created [`src/routes/ChildComponent.svelte`](/src/routes/ChildComponent.svelte) that gets the context containing the store, and subscribes to it.

Then, run with

```
yarn dev
```

and load the page.

# What is the bug?

Per [the documentation](https://svelte.dev/docs#run-time-svelte-store-derived) about the `derived` store, the callback should be evaluated lazily:

> The callback runs initially when the first subscriber subscribes and then whenever the store dependencies change.

However, in this repro I am seeing that on the backend, the callback is being evaluated for every instance of a subscriber.

```
calculating the derived store, attempt 1
calculating the derived store, attempt 2
calculating the derived store, attempt 3
calculating the derived store, attempt 4
```

Compare those console logs with those seen on the browser, which I would consider to be the correct behavior given the documentation, where there is only a single evaluation, at the time of the first subscription:

```
calculating the derived store, attempt 1
```

# What else did I try?

- This happens even if the store is a singleton in some other ts file
- This happens if the store is passed via context as opposed to exports
