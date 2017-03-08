# Feature Flag

http://stackoverflow.com/questions/7707383/what-is-a-feature-flag
There are heaps of other reasons you would want to use this tho - one of the main being enabling Continuous Delivery: pushing things into production/live yet having the feature disabled/toggled until it's completed. We often use what we call a 'dev cookie' to show uncompleted features to just the dev team. This way we can test partially completed work in production (oh yeh! is there better integration?) over multiple releases/deployments before we 'untoggle' (completed) it and it becomes visible to the public.


https://martinfowler.com/articles/feature-toggles.html

It can be tempting to lump all feature toggles into the same bucket, but this is a dangerous path. The design forces at play for different categories of toggles are quite different and managing them all in the same way can lead to pain down the road.

Feature toggles can be categorized across two major dimensions: `how long the feature toggle will live` and `how dynamic the toggling decision must be`. There are other factors to consider - who will manage the feature toggle, for example - but I consider longevity and dynamism to be two big factors which can help guide how to manage toggles.


"separating [feature] release from [code] deployment."