---
description: Customizing the Single Page Account UI
---

# Single-Page

## Initializing the Single-Page Account Theme

<figure><img src="../.gitbook/assets/image (66).png" alt=""><figcaption><p>The account Single Page Account theme before customization</p></figcaption></figure>

You've made your mind and opted for the Single Page Account UI?  \
Great, let's start by initializing you theme: &#x20;

```bash
npx keycloakify initialize-account-theme
```

<figure><img src="../.gitbook/assets/image (67).png" alt=""><figcaption><p>When prompted, select the "Single-Page" option</p></figcaption></figure>

This command will create the nessesary boilerpate and add to your project dependencies the required dependencies of the latest Account UI.

<figure><img src="../.gitbook/assets/image (68).png" alt=""><figcaption><p>Dependency added to your project when using initializing the Single-Page Account UI</p></figcaption></figure>

Before starting the customization, let's make sure that everything works by adding a simple console.log.

<pre class="language-tsx" data-title="src/account/KcPage.tsx"><code class="lang-tsx">import { lazy } from "react";
import { KcAccountUiLoader } from "@keycloakify/keycloak-account-ui";
import type { KcContext } from "./KcContext";

const KcAccountUi = lazy(() => import("@keycloakify/keycloak-account-ui/KcAccountUi"));

export default function KcPage(props: { kcContext: KcContext }) {
    const { kcContext } = props;
    
<strong>    console.log("This is my account theme!");
</strong>    
    return &#x3C;KcAccountUiLoader kcContext={kcContext} KcAccountUi={KcAccountUi} />;
}
</code></pre>

Then let's start Keycloak and test it live:

```bash
npx keycloakify start-keycloak
```

Selet Keycloak 25.

<figure><img src="../.gitbook/assets/image (69).png" alt="" width="375"><figcaption><p>Keycloak 25 up and runing on your computer</p></figcaption></figure>

Once you get the confirmation message that Keycloak is up and running you can reach the https://my-theme.keycloakify.dev to get redirected to your Login theme.\
Use the test credentials to authenticate as the test user:

<figure><img src="../.gitbook/assets/image (70).png" alt="" width="375"><figcaption><p>Authenticating as the test user</p></figcaption></figure>

On the next page you'll be provided with a link to the Account pages:

<figure><img src="../.gitbook/assets/image (71).png" alt=""><figcaption><p>Authenticated as "testuser" on the my-theme.keycloakify.dev utility app</p></figcaption></figure>

You should be able to see your log statement ([printed twice](#user-content-fn-1)[^1]), confirming that you are indeed running your theme.

<figure><img src="../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

Compilation of your theme is running in watch mode when using the start-keycloak command, you can eddit your console.log message, save and after a few seconds reload the page, you should see the message updated.\
\
After completing the initialization process, it's a good time to commit the changes.

```bash
git add -A
git commit -am "Initialize the Single-Page Account Theme"
```

## Basic customization

You can customize some aspect of the account theme witout having to go down at the React component level.  \
If you stick to this level of customization the Keycloak team is able to guarenty that you'll be able to keep your theme compatible with upcoming version of Keycloak with minimal maintenance effort. &#x20;

Let's see what we can do.

### Changing the logo

First start by adding a logo file in your src directory somewhere, example:&#x20;

<figure><img src="../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

And pass a reference to it as props of the `<KcAccountUiLoader />` component:

<pre class="language-tsx" data-title="src/account/KcPage.tsx"><code class="lang-tsx">import { lazy } from "react";
import { KcAccountUiLoader } from "@keycloakify/keycloak-account-ui";
import type { KcContext } from "./KcContext";
<strong>import myLogoPngUrl from "./assets/my-logo.png";
</strong>
const KcAccountUi = lazy(() => import("@keycloakify/keycloak-account-ui/KcAccountUi"));

export default function KcPage(props: { kcContext: KcContext }) {
    const { kcContext } = props;

    return (
        &#x3C;KcAccountUiLoader
            kcContext={kcContext}
            KcAccountUi={KcAccountUi}
<strong>            logoUrl={myLogoPngUrl}
</strong>        />
    );
}
</code></pre>

After saving and reloading you should be able to see that the logo has been updated:

<figure><img src="../.gitbook/assets/image (75).png" alt="" width="375"><figcaption><p>The Keycloak logo has been replaced by the Keycloakify logo</p></figcaption></figure>

### Customizing what sections are availables in the left pannel

Beside changing option in your Keycloak realm configuration you can define what options should be displayed and when in the left pannel.  \
For that you can use the content props of the  `KcAccountUiLoader` component. &#x20;

You can start by copy/pasting the default content located in **node\_modules/@keycloakify/keycloak-account-ui/src/public/content.ts**

<pre class="language-tsx" data-title="src/account/KcPage.tsx"><code class="lang-tsx">import { lazy } from "react";
import { KcAccountUiLoader } from "@keycloakify/keycloak-account-ui";
import type { KcContext } from "./KcContext";
import myLogoPngUrl from "./assets/my-logo.png";

const KcAccountUi = lazy(() => import("@keycloakify/keycloak-account-ui/KcAccountUi"));

export default function KcPage(props: { kcContext: KcContext }) {
    const { kcContext } = props;

    return (
        &#x3C;KcAccountUiLoader
            kcContext={kcContext}
            KcAccountUi={KcAccountUi}
            logoUrl={myLogoPngUrl}
<strong>            content={[
</strong><strong>                {
</strong><strong>                    label: "personalInfo",
</strong><strong>                    path: ""
</strong><strong>                },
</strong><strong>                {
</strong><strong>                    label: "accountSecurity",
</strong><strong>                    children: [
</strong><strong>                        {
</strong><strong>                            label: "signingIn",
</strong><strong>                            path: "account-security/signing-in"
</strong><strong>                        },
</strong><strong>                        {
</strong><strong>                            label: "deviceActivity",
</strong><strong>                            path: "account-security/device-activity"
</strong><strong>                        },
</strong><strong>                        {
</strong><strong>                            label: "linkedAccounts",
</strong><strong>                            path: "account-security/linked-accounts",
</strong><strong>                            isVisible: "isLinkedAccountsEnabled"
</strong><strong>                        }
</strong><strong>                    ]
</strong><strong>                },
</strong><strong>                {
</strong><strong>                    label: "applications",
</strong><strong>                    path: "applications"
</strong><strong>                },
</strong><strong>                {
</strong><strong>                    label: "groups",
</strong><strong>                    path: "groups",
</strong><strong>                    isVisible: "isViewGroupsEnabled"
</strong><strong>                },
</strong><strong>                {
</strong><strong>                    label: "resources",
</strong><strong>                    path: "resources",
</strong><strong>                    isVisible: "isMyResourcesEnabled"
</strong><strong>                },
</strong><strong>                {
</strong><strong>                    label: "oid4vci",
</strong><strong>                    path: "oid4vci",
</strong><strong>                    isVisible: "isOid4VciEnabled"
</strong><strong>                }
</strong><strong>            ]}
</strong>        />
    );
}
</code></pre>

Let's for example comment out the deviceActivity section.

<figure><img src="../.gitbook/assets/image (76).png" alt="" width="297"><figcaption><p>Before</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (77).png" alt="" width="375"><figcaption><p>Commenting out deviceActivity</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (78).png" alt="" width="315"><figcaption><p>After, the Device Activity section has disapeared</p></figcaption></figure>

### Adding custom CSS

The official way of customizing the look of the Account UI is to overload the PaternFly CSS variables:&#x20;

{% embed url="https://www.patternfly.org/components/button/html/#css-variables" %}

But we are aware that this isn't enough for most of you. &#x20;

Beyond that you can of course import your custom CSS and use the PaternFly utility classes as target but be aware that theses classes can be changed in the future.

\
Example loading some custom CSS:

<figure><img src="../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (80).png" alt=""><figcaption><p>The red boder has been applied on all the element that have the pf-v5-c-page__main-secrion class</p></figcaption></figure>

## Component level customization

To customize the Account theme at the React component level you want to use the eject-page command and slect "account".

```bash
npx keycloakify eject-page
```

<figure><img src="../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure>

After running this command you'll be able to see that the following change has been automatically applied:

{% code title="src/account/KcPage.tsx" %}
```diff
 import { lazy } from "react";
 import { KcAccountUiLoader } from "@keycloakify/keycloak-account-ui";
 import type { KcContext } from "./KcContext";

-const KcAccountUi = lazy(()=> import("@keycloakify/keycloak-account-ui/KcAccountUi"));
+const KcAccountUi = lazy(() => import("./KcAccountUi"));

 export default function KcPage(props: { kcContext: KcContext }) {
     const { kcContext } = props;

     return (
         <KcAccountUiLoader
             kcContext={kcContext}
             KcAccountUi={KcAccountUi}
         />
     );
 }
```
{% endcode %}

Also the KcAccountUi component has be copied over from **node\_modules/@keycloakify/keycloak-account-ui/src/KcAccountUi.tsx** to your codebase at **src/account/KcAccountUi.tsx**. &#x20;

<figure><img src="../.gitbook/assets/image (82).png" alt=""><figcaption><p>KcAccountUi.tsx has been ejected</p></figcaption></figure>

That what you'll want to each time you'll want to take ownership of some component of the Account UI: Copy the original source file into your codebase and update the absolute imports by relative imports.  \
Let's see in practice how we would eject the routes:

```bash
cp node_modules/@keycloakify/keycloak-account-ui/src/routes.tsx src/account/
```

Then you want to update the absolututes import of the routes to your local routes in KcAccountUI.tsx

```diff
- import { routes } from "@keycloakify/keycloak-account-ui/routes";
+ import { router } from "./routes";
```

<figure><img src="../.gitbook/assets/image (83).png" alt=""><figcaption></figcaption></figure>

And that's it, you now own the routes!  \
Moving forward you can incrementally "eject" the components you need to take ownerhip over by followind the same process. &#x20;

Be aware: The more components you eject, the more work it will represent to mainain your theme up to date with the future evolution of Keycloak.  &#x20;

## Updating keycloak-account-ui to a new version

Each time a new version of Keycloak is released, a new version of @keycloak/keycloak-account-ui is also released with the same version number.\
This does not nessesarely means that in order to have a custom theme that works with Keycloak 25.0.2 for example you need to use @keycloak/keycloak-account-ui@25.0.2. Actually it's very likely that you theme will still work with Keycloak 26, however at some point your theme will break with future version of Keycloak and you'll have to upgrade. &#x20;

Keycloakify does not use the NPM package @keycloak/keycloak-account-ui but an automatically repackaged distribution of it: @keycloakify/keycloak-account-ui.

So, when comes the time to upgrade you want to navigate to:\


{% embed url="https://github.com/keycloakify/keycloak-account-ui" %}

And look in the README in the installation section:

<figure><img src="../.gitbook/assets/image (84).png" alt=""><figcaption><p>Instalation section of keycloakify/keycloak-account-ui version 25.0.2</p></figcaption></figure>

You want to copy and paste the dependencies into the package.json of your Keycloakify project.

The readme is generated automatically, you can trust that is always up do date.

You migh wonder why there's only RC releases of @keycloakify/keycloak-account-ui, it's because we want to match the version number of the upstream package @keycloak/keycloak-account-ui but still be able to publish update when minor changes on the re-packaging distribution is needed. &#x20;

[^1]: React, in developement mode, when using `<StrictMode/>`, renders the components twice, this is not a bug.