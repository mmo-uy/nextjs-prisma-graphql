### Prisma commands
> ``yarn add -D prisma``

> `` npx prisma init ``

> ``npx prisma migrate dev --name init``

> ``npx prisma db seed``

> ``npx prisma generate``

> ``npx prisma studio``

### Auth0
#### Actions > Flow > Custom action:
##### Login / Post Login

    const fetch = require('node-fetch')
    
    exports.onExecutePostLogin = async (event, api) => {
	    const SECRET = event.secrets.AUTH0_HOOK_SECRET
	    const URL = event.secrets.URL
	    if (event.user.app_metadata.localUserCreated) {
		    return
	    }
	    const email = event.user.email
	    const request = await fetch('$URL/api/auth/hook', {
		    method: 'post',
		    body: JSON.stringify({ email, secret: SECRET }),
		    headers: { 'Content-Type': 'application/json' },
	    })
	    const response = await request.json()
	    api.user.setAppMetadata('localUserCreated', true)
    }
![Custom Action](https://www.prisma.io/blog/blog/posts/fullstack-nextjs-graphql-prisma-3/auth0-action-customize-login-flow.png)