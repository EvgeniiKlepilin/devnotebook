# React
## Troubleshooting
### Waiting for state update with useState hook
#### Issue
React state updates are asynchronously processed, but the state updater function itself isn't `async` so you can't wait for the update to happen. You can only ever access the state value from the current render cycle. This is why images is likely still your initial state, an empty array (`[]`).
#### Solution
I think you should rethink how you compute the next state of images, do a single update, and then use an `useEffect` hook to dispatch the action with the updated state value.
