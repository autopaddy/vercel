## What is the purpose of this fork?
`vercel dev` was serving files to the client very slowly.  In my fairly small project it was taking anywhere between ~45s-1m30s before my application was interactable.   This had overhead on development so I wanted to address it.

This change has brought my time to interactive down from ~45s-1m30s to <1s. 

## What has changed?
**This change is more of a workaround than a fix**.  The issue was: the `getVercelConfig()` in `packages/cli/src/util/dev/server.ts` was being called repeatadly when serving files and this method was blocking files from getting served to the client.  As part of `getVercelConfig()` it gets all the files in the current working directory and gets a relative path to the file;  this is what was holding up the command.

I say the change is more of a workaround as all I've done is cached the relative paths of the files in the current working directory so its not constantly getting files.  I suspect the real fix here is to stop `vercel dev` from making multiple calls to `getVercelConfig()` OR to make it so it doesn't block file serving.

## Usage
Instead of this:
1. `npm install -g vercel`

Do this:
1. `git clone https://github.com/autopaddy/vercel.git`
2. `cd vercel`
3. `yarn install && yarn build`
4. `cd ./packages/cli && yarn link`
