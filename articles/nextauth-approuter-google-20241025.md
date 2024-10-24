---
title: "Next.js AppRouter x NextAuth で Google ログインを実装してみる"
emoji: "📌"
type: "tech"
topics: ["nextjs", "nextauth", "approuter", "react", "typescript"]
published: true
---

# はじめに
今回は Next.js AppRouter で NextAuth を使用し、サクッと Google ログインを実装しようと思います。
というのも、個人開発で Youtube Data API v3 を呼ぶのに NextAuth でとれる access_token を使用しようと思ったところ、AppRouter で NextAuth を使用している記事があまり出てこずに苦労したのでここに備忘録として記述しておこうと思います。

# 環境
- next: 15.0.1
- next-auth 4.24.9

:::message alert
next-auth が 4.24.8 だと [sync-dynamic-apis](https://nextjs.org/docs/messages/sync-dynamic-apis) に関するエラーが出ます。
:::

# 必要な環境変数の設定
OAuth2 の `clientId`, `clientSecret` は各自 [Google cloud console](https://console.cloud.google.com/apis/dashboard) で準備して下さい。

プロジェクトルートに `.env` ファイルを作成し、必要な環境変数を記述する。
```plaintext:.env
NEXTAUTH_SECRET=
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
```

# next-auth の設定
`src/app/auth/[...nextauth]/route.ts` を作成します
```ts:src/app/auth/[...nextauth]/route.ts
import type { NextAuthOptions } from "next-auth";
import type { DefaultSession } from "next-auth";
import NextAuth from "next-auth/next";

// 変数名が options だと警告が出たから OPTIONS
export const OPTIONS: NextAuthOptions = {
	session: {
		strategy: "jwt",
	},
	providers: [
		GoogleProvider({
			clientId: process.env.GOOGLE_CLIENT_ID as string,
			clientSecret: process.env.GOOGLE_CLIENT_SECRET as string,
			authorization: {
				params: {
					scope: [
                        // API を NextAuth からのトークンでたたきたいならここでスコープを設定する。
                        // 認証だけならおそらくいらない
						"https://www.googleapis.com/auth/youtube", 
					].join(" "), // スコープは半角スペース区切りの文字列で与える
				},
			},
		}),
	]
};

const handler = NextAuth(OPTIONS);
export { handler as GET, handler as POST };
```

# NextAuth のトークンを別の場所でも利用できるようにする
API 呼び出しにも NextAuth からのトークンを使用するための変更。
```diff ts:src/app/auth/[...nextauth]/route.ts
// セッションを取得するときに accessToken が含まれている型を使用するために declare
+ import type { DefaultSession } from "next-auth";

+ declare module "next-auth" {
+ 	interface Session extends DefaultSession {
+ 		accessToken?: string;
+ 		user: {
+ 			id: string;
+ 		} & DefaultSession["user"];
+ 	}
+ }

+ declare module "next-auth/jwt" {
+ 	interface JWT {
+ 		accessToken?: string;
+ 	}
+ }

export const OPTIONS: NextAuthOptions = {
	providers: [
        // ...
	],
+	callbacks: {
+		async jwt({ token, account }) {
+			if (account) {
+				token.accessToken = account.access_token;
+			}
+			return token;
+		},
+		async session({ session, token }) {
+			session.accessToken = token.accessToken;
+			return session;
+		},
+	},
};

// ...
```

# サインイン/サインアウトするには
`signIn`, `signOut` 関数を呼ぶだけで実行できます。
```tsx: GoogleLoginButton.tsx
"use client";

import { signIn } from "next-auth/react";

export const GoogleLoginButton = () => {
	const handleClick = async () => {
		await signIn("google");
	};

	return (
		<button type="button" onClick={handleClick}>
			Sign in with Google
		</button>
	);
};

```

# セッション情報を取得する
client component なら　[`useSession`](https://next-auth.js.org/getting-started/client#custom-client-session-handling) 
server component なら [`getServerSession`](https://next-auth.js.org/getting-started/example#backend---api-route) でセッション情報を取得することができます。
## access_token を API 呼び出しに使う
```tsx: anyComp.tsx
import { OPTIONS } from "@/app/api/auth/[...nextauth]/route";
import { getServerSession } from "next-auth/next";

export const AnyComponent = async () => {
    // OPTIONS を渡さないと OPTIONS.callbacks.session が呼ばれず
    // session に access_token が含まれません。
    const session = await getServerSession(OPTIONS);
    const token = session.accessToken;

    // API にリクエスト
};
```

# おわりに
今回は Next.js AppRouter x NextAuth について解説しました。
AppRouter での NextAuth 実装の情報が少なかった（自分調べ）ので、誰かの役に立てば幸いです。
今回の記事中に間違いや改善点があれば、コメントなどで教えてください！

最後に今回のコードを含んだ、現在開発中のウェブアプリケーションのリポジトリを載せておきます。
https://github.com/suzuki3jp/PlaylistManager