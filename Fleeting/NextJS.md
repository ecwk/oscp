- Server-centric routing
	- uses React Server components
	- client does not download route map like in nextjs 12
- Router uses in-memory client-side cache
	- Each route segment has its own cache
	- Hence, can invalidate any segment and ensure consistency, e.g. if data in one segment is invalidated, other segments using that data is also invalidated and will have to be reprocessed
	- Additionally, previously fetched segment data can be re-used to improve performance
- Because of cache, it allows partial rendering
	- When navigating to sibling or child routes with the same parent route, the parent will not be re-rendered including any data used in the parent
- All components under `/app` are React Server Components
	- Can opt out
- Order of rendering
	1. layout
	2. template
	3. error
	4. loading
	5. not found
	6. page

## React Server Components
- https://www.youtube.com/watch?v=TQQPAU21ZUw&ab_channel=MetaOpenSource
- Server renders components for client, e.g. uses formatting library to format dates (hence, packages arent sent on client and increases performance)
- Server components can render client components
	- React is smart enough to hybrid render, server components data will be rendered and client component jsx will be sent over network
	- client components only used if need interactivity
	- Alternatively to increase performance, if a client component needs interactivity but also requires jsx, you can create two components
	- One for jsx (server) or for interactivty. Then, pass the server jsx component into the child interactivity component as the children
- SSR vs Server Components 
	- SSR is for initial page load
	- Server Components can preserve client components, hence, allows use of server components during page navigation
- Hence, SSR and Server Components are complementary