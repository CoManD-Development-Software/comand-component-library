<template>
    <div v-if="boxType === 'content'" class="cmd-box box content">
        <div>
            <slot name="header"></slot>
        </div>
        <div class="box-body">
            <slot name="body"></slot>
        </div>
        <footer>
            <slot name="footer"></slot>
        </footer>
    </div>
    <a v-else-if="boxType === 'product'"  class="cmd-box box product" href="#" @click.prevent="">
        <div>
            <img v-if="product.img" :src="product.img.src" :alt="product.img.alt" />
            <div class="ribbon-new" v-if="product.new">
                <span>New</span>
            </div>
            <div class="ribbon-discount" v-if="product.discount">
                <span>{{ product.discount }}</span>
            </div>
        </div>
        <div class="box-body">
            <h3>{{ product.name }}</h3>
            <p v-if="product.articleNumber">Article No.: {{ product.articleNumber }}</p>
            <p v-if="product.price" class="price">{{ product.price }}</p>
            <p v-if="product.description">{{ product.description }}</p>
        </div>
    </a>
    <div v-else-if="boxType === 'user'" class="cmd-box box user">
        <div>
            <img v-if="user.image" src="" alt="" />
            <div v-else class="icon-user-profile"></div>
            <h3 v-if="user.name">{{ user.name }}</h3>
        </div>
        <div class="box-body">
            <p v-if="user.profession">{{ user.profession }}</p>
            <p v-if="user.position">{{ user.position }}</p>
            <p v-if="user.description" class="description">{{ user.description }}</p>
        </div>
        <footer v-if="user.links">
            <ul class="flex-container no-gap">
                <li v-for="(link, linkIndex) in user.links" :key="linkIndex">
                    <a :href="link.href" :target="link.target" :title="link.title" :class="link.iconClass"></a>
                </li>
            </ul>
        </footer>
    </div>
</template>

<script>
export default {
    name: "CmdBox",
    props: {
        boxType: {
          type: String,
          default: "content"
        },
        product: {
            type: Object,
            required: false
        },
        user: {
            type: Object,
            required: true
        }
    }
}
</script>

<style lang="scss">
/* begin cmd-box ---------------------------------------------------------------------------------------- */
.cmd-box {
    display: flex;
    flex-direction: column;
    height: 100%;
    padding: 0;

    .box-body {
        padding: var(--default-padding);
    }

    &.content {
        > * {
            > *:last-child {
                margin-bottom: 0;
            }

            &:last-child {
                margin-top: auto;
            }
        }

        > div:first-child {
            border-top-left-radius: var(--border-radius);
            border-top-right-radius: var(--border-radius);
            padding: calc(var(--default-padding) / 2) var(--default-padding);
            background: var(--primary-color);

            > * {
                color: var(--pure-white);
            }
        }

        .box-body {
            height: 100%;
            padding: 0;

            .padding {
                padding: var(--default-padding);
            }

            img {
                display: block;
            }

            .navigation {
                margin: 0;
                height: 100%;
                background: var(--pure-white);

                li {
                    list-style-type: none;
                    margin: 0;

                    a {
                        display: block;
                        padding: var(--default-padding);
                        text-decoration: none;
                        background: var(--pure-white);
                        border-bottom: var(--default-border);

                        &:hover, &:active, &:focus {
                            background: var(--primary-color);
                            color: var(--pure-white);
                        }
                    }

                    &:last-child {
                        a {
                            border: 0;
                        }
                    }
                }
            }
        }

        footer {
            border-bottom-left-radius: var(--border-radius);
            border-bottom-right-radius: var(--border-radius);
            padding: var(--default-padding);
            border-top: var(--default-border);
        }
    }

    &.product {
        text-decoration: none;
        overflow: hidden;

        [class*="ribbon"] {
            position: absolute;
            z-index: 10;
            overflow: hidden;
            width: 10rem;
            height: 10rem;
            top: 0;
            left: 0;

            span {
                position: absolute;
                display: block;
                transform: rotate(-45deg);
                width: 100%;
                top: 15%;
                left: -25%;
                background: var(--error-color);
                font-weight: bold;
                color: var(--pure-white);
                text-transform: uppercase;
                text-align: center;
                box-shadow: var(--box-shadow);
            }
        }

        .ribbon-discount {
            left: auto;
            right: 0;

            span {
                transform: rotate(45deg);
                right: -20%;
                left: auto;
                background: green;
            }
        }

        > div:first-child {
            > img, > div {
                display: table;
                margin: 0 auto;
            }
        }

        .box-body {
            > * {
                text-align: center;
            }

            .price {
                font-size: 2rem;
                font-weight: bold;
            }
        }

        &:hover, &:active, &:focus {
            border-color: var(--hyperlink-color-highlighted);
        }
    }

    &.user {
        > div:first-child {
            padding: var(--default-padding);
            background: var(--pure-white);

            > img, > div {
                display: table;
                margin: 0 auto;
                padding: calc(var(--default-padding) * 3);
                border-radius: var(--full-circle);
                background: var(--primary-color);
                font-size: 6rem;
                color: var(--pure-white);
            }

            h3 {
                margin-top: var(--default-margin);
                margin-bottom: 0;
                text-align: center;
            }
        }

        .box-body {
            p {
                text-align: center;
                font-weight: bold;

                &:last-child {
                    margin-bottom: 0;
                }

                &.description {
                    font-weight: normal;
                }
            }
        }

        footer {
            margin-top: auto;
            border-top: var(--default-border);

            ul {
                margin: 0;

                li {
                    flex: 1;
                    margin: 0;
                    list-style-type: none;

                    a {
                        display: block;
                        padding: var(--default-padding);
                        text-align: center;
                        background: var(--pure-white);
                        border-left: var(--default-border);


                    }

                    &:hover, &:active, &:focus {
                        a {
                            background: var(--primary-color);
                            color: var(--pure-white);
                        }
                    }

                    &:first-child {
                        a {
                            border: 0;
                        }
                    }
                }
            }
        }
    }
}
/* end cmd-box ------------------------------------------------------------------------------------------ */
</style>