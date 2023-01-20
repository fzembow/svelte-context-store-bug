# How was this repro created

First, I installed svelte kit

```
npm create svelte@latest store-bug
```

Then, I edited `src/routes/+page.svelte` to set a context containing a store.

Lastly, I created `src/routes/ChildComponent.svelte` that gets the context containing the store, and subscribes to it.

Then, run with

```
yarn dev
```

and load the page.

# What is the bug?

Per [the documentation](https://svelte.dev/docs#run-time-svelte-store-derived) about the `derived` store, the callback is evaluated lazily:

> The callback runs initially when the first subscriber subscribes and then whenever the store dependencies change.

However, in this repro I am seeing that on the backend, the callback is being evaluated for every instance of a subscriber.

```
setting the context in the parent
getting the context in component 1
calculating the derived store, attempt 1
getting the context in component 2
calculating the derived store, attempt 2
getting the context in component 3
calculating the derived store, attempt 3
getting the context in component 4
calculating the derived store, attempt 4
```

Compare those console logs with those seen on the browser, which I would consider to be the correct behavior given the documentation, where there is only a single evaluation, at the time of the first subscription:

```
setting the context in the parent
ChildComponent.svelte:7 getting the context in component 1
+page.svelte:16 calculating the derived store, attempt 1
ChildComponent.svelte:7 getting the context in component 2
ChildComponent.svelte:7 getting the context in component 3
ChildComponent.svelte:7 getting the context in component 4
```

# Why am I putting a store in a context?

It's to workaround the problem discussed in https://github.com/sveltejs/kit/discussions/4339, where if there were a singleton store on the server then it would be shared across all requests.

In my particular app, the derived calculation is somewhat intensive, and I don't want it running multiple times on the server unnecessarily.
