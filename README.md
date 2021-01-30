# Prismap

Prismap, which stands for Prisma Map and Pluralize, automatically maps and pluralizes Prisma schema to match conventional database names with JavaScript/TypeScript naming conventions.

## Key Feautres

* Map table names to PascalCase.
* Map attribute names to camelCase.
* Pluralize 1:m, n:m relations.

### Example

```prisma
model user {
  id             Int              @id
  nickname       String           @unique
  joined_at      DateTime
  community_post community_post[]
}

model community_post {
  id      Int    @id
  body    String
  user_id Int
  user    user   @relation(fields: [user_id], references: [id])
}
```

converts to

```prisma
model User {
  id             Int             @id
  nickname       String          @unique
  joinedAt       DateTime        @map(name: "joined_at")
  communityPosts CommunityPost[]

  @@map(name: "user")
}

model CommunityPost {
  id     Int    @id
  body   String
  userId Int    @map(name: "user_id")
  user   User   @relation(fields: [userId], references: [id])

  @@map(name: "community_post")
}
```

**PascalCase table names**

```diff
- model User {
-   ...
- }
+ model user {
+   ...
+   @@map(name: "user")
+ }
```

```diff
- model community_post {
-   ...
- }
+ model CommunityPost {
+   ...
+   @@map(name: "community_post")
+ }
```

**camelCase attribute names**

```diff
- joined_at DateTime
+ joinedAt  DateTime @map(name: "joined_at")
```

```diff
- user_id Int
+ userId  Int @map(name: "user_id")
```

**Pluralize relations**

```diff
- community_post community_post[]
+ communityPosts CommunityPost[]
```

## Installation

```zsh
npm i -g prismap
```

## Usage

Inside your project directory, run

```zsh
prismap
```

### Options

**`--schema`**

Specify the Prisma schema path. Prismap finds `prisma/schema.prisma` in default.
