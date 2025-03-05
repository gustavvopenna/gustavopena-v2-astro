---
title: "Using TanStack Query with GraphQL in React & Next.js"
description: "A practical guide to integrating TanStack Query with GraphQL in React and Next.js projects, improving data handling and developer experience."
date: "Feb 22 2025"
---

When working with GraphQL in a React and Next.js project, managing queries efficiently is crucial. TanStack Query (formerly React Query) simplifies data fetching, caching, and synchronization, making it an excellent choice for handling GraphQL requests.

## Initial Setup

First, ensure you have a Next.js project ready. If not, create one with:

```sh
npx create-next-app@latest my-graphql-app
cd my-graphql-app
```

Then, install the necessary dependencies:

```sh
npm install @tanstack/react-query graphql-request graphql
```

## Configuring TanStack Query

Create a `queryClient.ts` file to set up the Query Client:

```ts
import { QueryClient } from '@tanstack/react-query';

export const queryClient = new QueryClient();
```

Next, wrap your application with the Query Client Provider in `_app.tsx`:

```tsx
import { QueryClientProvider } from '@tanstack/react-query';
import { queryClient } from '../lib/queryClient';
import type { AppProps } from 'next/app';

export default function MyApp({ Component, pageProps }: AppProps) {
  return (
    <QueryClientProvider client={queryClient}>
      <Component {...pageProps} />
    </QueryClientProvider>
  );
}
```

## Creating a GraphQL Query Hook

We'll use `graphql-request` to fetch data from a GraphQL API. Create a `usePosts.ts` file:

```ts
import { useQuery } from '@tanstack/react-query';
import { request, gql } from 'graphql-request';

const API_URL = 'https://your-graphql-api.com/graphql';

const GET_POSTS = gql`
  query GetPosts {
    posts {
      id
      title
      content
    }
  }
`;

const fetchPosts = async () => {
  return request(API_URL, GET_POSTS);
};

export function usePosts() {
  return useQuery({ queryKey: ['posts'], queryFn: fetchPosts });
}
```

## Using the Hook in a Component

Now, let's use the `usePosts` hook in a Next.js page to display data:

```tsx
import { usePosts } from '../hooks/usePosts';

export default function Home() {
  const { data, error, isLoading } = usePosts();

  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return (
    <div>
      <h1>Posts</h1>
      <ul>
        {data.posts.map((post: any) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}
```

## Benefits of This Implementation

1. **Efficient Cache Management**: TanStack Query automatically handles data caching.
2. **Real-time Updates**: Easy configuration for revalidation and polling.
3. **State Management**: Loading and error states handled automatically.
4. **Performance Optimizations**: Includes request deduplication and automatic retries.

## Conclusion

Using TanStack Query with GraphQL in a Next.js project significantly improves data fetching, caching, and error handling while keeping the code clean and maintainable. By leveraging `graphql-request`, we simplify GraphQL queries without the complexity of larger GraphQL clients like Apollo.

This setup provides you with a solid foundation for building efficient and scalable React applications with GraphQL. 🚀 