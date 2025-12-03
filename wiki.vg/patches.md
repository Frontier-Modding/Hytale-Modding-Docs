# Game Patches

Hytale game patches use the [Wharf](https://itch.io/docs/wharf/) specification by [Itch.io](https://itch.io). You could therefore, in theory, use their CLI tools like [butler](https://itch.io/docs/butler) to [interact with patch files](https://itch.io/docs/butler/offline.html) and signatures once you've downloaded them.

> [!TIP]
>
> The Hytale Launcher always keeps one previous build version around, so you can always switch back to whatever version you had previously, should the current one be broken.



### Patching the game

The [`patches` endpoint](api.md#Game%20Updates) provides download URLs for patch files to get from the `current` local build (or `0` if you have none), to the `latest` build. It is not possible to pick a specific build and upload only to that.

These updates are delta updates, and can be used to update ex. from build `7` to build `8`.
Note that for very old versions, there are larger delta patches to save on bandwidth: the API will return a patch to update to the next multiple of 5, then patch in multiples of 5 until you are close to the current version. Then finally 1 version increments to get to the final build.

**Example:** You are on build `3` and the current build is `17`. The api would provide you delta updates to get from build `3 -> 5`, then `5 -> 10`, `10 -> 15`, and finally `15 -> 16` and `16 -> 17`.

However if you dont have the game installed yet, (or presumably if you are extremely far behind), you will be provided a single patch file `0 -> 17`.

