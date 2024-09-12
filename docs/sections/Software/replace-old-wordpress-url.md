# Use the following script to replace your old wordpress url with a new one after migration

## Step 1
Replace ```https://youroldurl.com``` with your actual url of the old site
Replace ```https://yournewurl.com``` with your actual url of the new site

Note: Ensure you use the correct scheme (https or http)

## Step 2
Simulate the query on PHP MyAdmin to have an idea of what the query will do

## Step 3
Execute the query

Note: if the query fails with table not found, 
ensure the table prefix ie "wp_" is correct. This could be different depending on how you installed wordpress


```sql
UPDATE wp_options SET option_value = replace(option_value, 'https://youroldurl.com', 'https://yournewurl.com') WHERE option_name = 'home' OR option_name = 'siteurl';
UPDATE wp_posts SET guid = replace(guid, 'https://youroldurl.com','https://yournewurl.com');
UPDATE wp_posts SET post_content = replace(post_content, 'https://youroldurl.com', 'https://yournewurl.com');
UPDATE wp_postmeta SET meta_value = replace(meta_value,'https://youroldurl.com','https://yournewurl.com')
UPDATE wp_options SET option_value = replace(option_value, 'https://youroldurl.com', 'https://yournewurl.com') WHERE option_name = 'home' OR option_name = 'siteurl';
UPDATE wp_posts SET guid = replace(guid, 'https://youroldurl.com','https://yournewurl.com');
UPDATE wp_posts SET post_content = replace(post_content, 'https://youroldurl.com', 'https://yournewurl.com');
UPDATE wp_postmeta SET meta_value = replace(meta_value,'https://youroldurl.com','https://yournewurl.com');
```


With love https://acceptance.co.ke
