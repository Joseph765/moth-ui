<script>
    import { Search } from "@lucide/svelte";
    import * as Moth from "moth-ui-svelte";
    import { Badge, Code, Container, Flex, Link, Text } from "moth-ui-svelte";
    import * as Lit from "./ui/index.js";
    import Seo from "../Seo.svelte";

    const versions = [
        { label: "Current", ui: Moth },
        { label: "Lit", ui: Lit }
    ];

    const rows = [
        { name: "Ava Martinez", plan: "Team", amount: "$240.00" },
        { name: "Liam Chen", plan: "Pro", amount: "$29.00" },
        { name: "Noah Patel", plan: "Team", amount: "$480.00" }
    ];

    /** @type {('neutral' | 'info' | 'success' | 'warning' | 'danger')[]} */
    const badgeVariants = ["neutral", "info", "success", "warning", "danger"];

    /** @type {{ variant: 'info' | 'warning' | 'danger' | 'success', text: string }[]} */
    const banners = [
        { variant: "info", text: "A new version is available." },
        { variant: "warning", text: "Your card expires next month." },
        { variant: "danger", text: "The last payment failed." },
        { variant: "success", text: "Your changes were saved." }
    ];

    const tokens = [
        {
            name: "--m-gradient-surface",
            use: "background-image for cards and tables. Dark: lighter at the top, gone by 160px. Light: settles slightly darker towards the bottom."
        },
        {
            name: "--m-shadow-surface",
            use: "box-shadow for cards and tables. Dark: a 1px highlight on the top edge. Light: a soft shadow underneath."
        },
        {
            name: "--m-color-surface-raised-tint",
            use: "background-color inside a card or table (the table header). A see-through --m-color-surface-raised: one step up from whatever it sits on."
        },
        {
            name: "--m-shadow-field",
            use: "box-shadow for form fields at rest. They're set into the surface, so the light from above leaves a soft shadow under their top edge."
        }
    ];
</script>

<Seo
    title="Experimental · Moth UI"
    description="Internal page for trying experimental styles."
    path="/experimental"
    noindex
/>

<Container align="center">
    <Flex direction="column" gap="2xl">
        <Flex direction="column" gap="xs">
            <Text size="3xl" weight="black" as="h1">Experimental</Text>
            <Text variant="weak">
                Lit surfaces: containers look lit from above, with a soft gradient and a highlight along the
                top edge. See it in an app on the <Link href="/experimental/dashboard">dashboard</Link>.
            </Text>
        </Flex>

        <Flex direction="column" gap="m">
            <Text size="2xl" weight="bold" as="h2" id="tokens">Surface tokens</Text>
            <div class="token-list">
                {#each tokens as token}
                    <Flex direction="column" gap="xs">
                        <Code>{token.name}</Code>
                        <Text size="s" variant="weak">{token.use}</Text>
                    </Flex>
                {/each}
            </div>
        </Flex>

        <Flex direction="column" gap="m">
            <Text size="2xl" weight="bold" as="h2" id="button">Button</Text>
            <Text variant="weak">
                Primary and danger get a gradient and a highlight along the top edge. Secondary, tertiary
                and disabled stay flat. On hover, more light shines on the top of primary and danger
                buttons; the label stays white.
            </Text>
            <div class="compare">
                {#each versions as { label, ui }}
                    <Flex direction="column" gap="s">
                        <Text size="s" variant="weaker">{label}</Text>
                        <Flex direction="column" gap="m">
                            <Flex gap="s" wrap>
                                <ui.Button variant="primary">Primary</ui.Button>
                                <ui.Button variant="secondary">Secondary</ui.Button>
                                <ui.Button variant="tertiary">Tertiary</ui.Button>
                                <ui.Button variant="danger">Danger</ui.Button>
                            </Flex>
                            <Flex gap="s" wrap align="center">
                                <ui.Button variant="primary" size="l">Large</ui.Button>
                                <ui.Button variant="danger" size="l">Large danger</ui.Button>
                            </Flex>
                            <Flex gap="s" wrap>
                                <ui.Button variant="primary" disabled>Disabled</ui.Button>
                                <ui.Button variant="secondary" disabled>Disabled</ui.Button>
                                <ui.Button variant="danger" disabled>Disabled</ui.Button>
                            </Flex>
                        </Flex>
                    </Flex>
                {/each}
            </div>
        </Flex>

        <Flex direction="column" gap="m">
            <Text size="2xl" weight="bold" as="h2" id="badge">Badge</Text>
            <Text variant="weak">
                Soft status badges are lit like a banner, strong ones like a button. Neutral badges stay
                flat.
            </Text>
            <div class="compare">
                {#each versions as { label, ui }}
                    <Flex direction="column" gap="s">
                        <Text size="s" variant="weaker">{label}</Text>
                        <Flex direction="column" gap="s">
                            <Flex gap="s" wrap>
                                {#each badgeVariants as variant}
                                    <ui.Badge {variant}>{variant}</ui.Badge>
                                {/each}
                            </Flex>
                            <Flex gap="s" wrap>
                                {#each badgeVariants as variant}
                                    <ui.Badge {variant} strong>{variant}</ui.Badge>
                                {/each}
                            </Flex>
                        </Flex>
                    </Flex>
                {/each}
            </div>
        </Flex>

        <Flex direction="column" gap="m">
            <Text size="2xl" weight="bold" as="h2" id="banner">Banner</Text>
            <Text variant="weak">Lit like a card, but the light takes the banner's own color.</Text>
            <div class="compare">
                {#each versions as { label, ui }}
                    <Flex direction="column" gap="s">
                        <Text size="s" variant="weaker">{label}</Text>
                        <Flex direction="column" gap="s">
                            {#each banners as banner}
                                <ui.Banner variant={banner.variant}
                                    >{banner.text} <Link href="#banner">Learn more.</Link></ui.Banner
                                >
                            {/each}
                        </Flex>
                    </Flex>
                {/each}
            </div>
        </Flex>

        <Flex direction="column" gap="m">
            <Text size="2xl" weight="bold" as="h2" id="input">Input</Text>
            <Text variant="weak">
                Set into the surface: under the light from above, its top edge casts a soft shadow inside
                it. On focus it lights up instead, as now.
            </Text>
            <div class="compare">
                {#each versions as { label, ui }}
                    {@const key = label.toLowerCase()}
                    <Flex direction="column" gap="s">
                        <Text size="s" variant="weaker">{label}</Text>
                        <ui.Card space="l">
                            <Flex direction="column" gap="l">
                                <ui.Input
                                    name="example-{key}-email"
                                    label="Email"
                                    placeholder="you@example.com"
                                    hint="We'll never share it."
                                    expanded
                                />
                                <ui.Input name="example-{key}-search" placeholder="Search orders" expanded>
                                    {#snippet prefix()}<Search size={16} />{/snippet}
                                </ui.Input>
                                <ui.Input
                                    name="example-{key}-username"
                                    label="Username"
                                    value="moth"
                                    error="That username is taken."
                                    expanded
                                />
                                <ui.Input
                                    name="example-{key}-disabled"
                                    label="Disabled"
                                    placeholder="Not editable"
                                    disabled
                                    expanded
                                />
                            </Flex>
                        </ui.Card>
                    </Flex>
                {/each}
            </div>
        </Flex>

        <Flex direction="column" gap="m">
            <Text size="2xl" weight="bold" as="h2" id="card">Card</Text>
            <Text variant="weak"
                >The card gets the light. A card nested in a lit card is <Code>flat</Code>.</Text
            >
            <div class="compare">
                {#each versions as { label, ui }}
                    <Flex direction="column" gap="s">
                        <Text size="s" variant="weaker">{label}</Text>
                        <ui.Card space="l">
                            {#snippet header()}<Text size="l" weight="bold" as="h3">Revenue</Text>{/snippet}
                            {#snippet headerEnd()}<Badge>8 weeks</Badge>{/snippet}
                            <Flex direction="column" gap="s">
                                <Text size="2xl" weight="bold">$48,290</Text>
                                <Flex gap="xs" align="center">
                                    <Badge variant="success">+12.4%</Badge>
                                    <Text size="s" variant="weaker">vs. previous period</Text>
                                </Flex>
                            </Flex>
                        </ui.Card>
                    </Flex>
                {/each}
            </div>
        </Flex>

        <Flex direction="column" gap="m">
            <Text size="2xl" weight="bold" as="h2" id="table">Table</Text>
            <Text variant="weak"
                >The wrapper gets the light; the header is a see-through step up, so it stays raised over
                the gradient.</Text
            >
            <div class="compare">
                {#each versions as { label, ui }}
                    <Flex direction="column" gap="s">
                        <Text size="s" variant="weaker">{label}</Text>
                        <ui.Table interactive>
                            <ui.TableHead>
                                <ui.TableRow>
                                    <ui.TableHeader>Customer</ui.TableHeader>
                                    <ui.TableHeader>Plan</ui.TableHeader>
                                    <ui.TableHeader align="end">Amount</ui.TableHeader>
                                </ui.TableRow>
                            </ui.TableHead>
                            <ui.TableBody>
                                {#each rows as row}
                                    <ui.TableRow>
                                        <ui.TableCell>{row.name}</ui.TableCell>
                                        <ui.TableCell>{row.plan}</ui.TableCell>
                                        <ui.TableCell align="end"><Text size="s" mono>{row.amount}</Text></ui.TableCell>
                                    </ui.TableRow>
                                {/each}
                            </ui.TableBody>
                        </ui.Table>
                    </Flex>
                {/each}
            </div>
        </Flex>
    </Flex>
</Container>

<style>
    .token-list {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(16rem, 1fr));
        gap: var(--m-space-l);
    }

    .compare {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(20rem, 1fr));
        gap: var(--m-space-l);
    }
</style>
