<template>
    <Layout>
        <template #main-content>
            <header>
                <div
                    class="max-w-xl md:max-w-3xl xl:max-w-4xl flex flex-col-reverse mx-auto text-center px-6 pt-24 pb-10 md:py-32 border-b border-gray-300"
                >
                    <p class="dark:text-gray-100 text-gray-700 leading-normal">
                        {{ $page.category.belongsTo.totalCount }} posts in total
                    </p>
                    <h1
                        class="dark:text-gray-100 text-4xl sm:text-5xl md:text-6xl font-sans font-bold mb-2 capitalize"
                    >
                        {{ titleCase($page.category.title) }}
                    </h1>
                    <svg
                        aria-labelledby="tagIcon"
                        class="dark:text-gray-100 w-5 sm:w-6 fill-current text-gray-500 mx-auto mb-1"
                        role="img"
                        viewBox="0 0 20 20"
                        xmlns="http://www.w3.org/2000/svg"
                    >
                        <title id="tagIcon">Posts tagged</title>
                        <path
                            d="M0 10V2l2-2h8l10 10-10 10L0 10zm4.5-4a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3z"
                        />
                    </svg>
                </div>
            </header>
            <section>
                <post-card
                    :key="edge.node.id"
                    :post="edge.node"
                    v-for="edge in $page.category.belongsTo.edges"
                />
            </section>
        </template>
    </Layout>
</template>

<script>
import formatService from '@/services/posts/format.service'
export default {
    name: 'Category',
    methods: {
        titleCase(str) {
            return formatService.toTitleCase(str)
        },
    },
}
</script>
<page-query>
query Category($path: String!, $page: Int) {
    category(path: $path) {
        id
        title
        path
        belongsTo(page: $page, perPage: 6) @paginate {
            totalCount
            pageInfo {
                totalPages
                currentPage
            }
            edges {
                node {
                    ... on Post {
                        id
                        title
                        date(format: "D MMMM Y")
                        path
                        content
                        excerpt
                        summary
                        description
                        timeToRead
                        categories {
                            id
                            title
                            path
                        }
                    }
                }
            }
        }
    }
}
</page-query>
