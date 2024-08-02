To set up a CI/CD pipeline for deploying a Zola static site to Netlify, follow these steps:

### Step 1: Set Up Your GitHub Repository
Ensure your Zola site is in a GitHub repository. If it's not, create a new repository and push your site code there.

### Step 2: Create a Netlify Account
1. Go to [Netlify](https://www.netlify.com/) and sign up for an account.
2. After signing up, click on "New site from Git" and connect your GitHub account.
3. Select the repository containing your Zola site.

### Step 3: Configure the Build Settings on Netlify
1. **Build command:** `zola build`
2. **Publish directory:** `public`

### Step 4: Add Netlify Configuration File
Create a `netlify.toml` file in the root of your repository with the following content:

```toml
[build]
  command = "zola build"
  publish = "public"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

### Step 5: Install Zola on Netlify Build Environment
Netlify allows you to install additional software in the build environment. You can use a script to install Zola during the build process.

1. Create a script named `install_zola.sh` in the root of your repository with the following content:

    ```bash
    #!/bin/bash

    ZOLA_VERSION="0.16.1"  # Replace with the latest version of Zola
    wget "https://github.com/getzola/zola/releases/download/v${ZOLA_VERSION}/zola-v${ZOLA_VERSION}-x86_64-unknown-linux-gnu.tar.gz"
    tar -xzf "zola-v${ZOLA_VERSION}-x86_64-unknown-linux-gnu.tar.gz"
    sudo mv zola /usr/local/bin/zola
    ```

2. Make the script executable:

    ```bash
    chmod +x install_zola.sh
    ```

3. Add a command to run this script in your `netlify.toml`:

    ```toml
    [build]
      command = "./install_zola.sh && zola build"
      publish = "public"
    ```

### Step 6: Push Changes to GitHub
Push all changes (including `netlify.toml` and `install_zola.sh`) to your GitHub repository.

### Step 7: Set Up Continuous Deployment on Netlify
1. Go to your site settings on Netlify.
2. Under "Build & deploy", ensure the settings match your `netlify.toml` file.
3. Connect your site to the GitHub repository.

### Step 8: Trigger Your First Build
Push a commit to the main branch of your GitHub repository to trigger a build on Netlify.

### Final `netlify.toml` Example

```toml
[build]
  command = "./install_zola.sh && zola build"
  publish = "public"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

### Final `install_zola.sh` Example

```bash
#!/bin/bash

ZOLA_VERSION="0.16.1"  # Replace with the latest version of Zola
wget "https://github.com/getzola/zola/releases/download/v${ZOLA_VERSION}/zola-v${ZOLA_VERSION}-x86_64-unknown-linux-gnu.tar.gz"
tar -xzf "zola-v${ZOLA_VERSION}-x86_64-unknown-linux-gnu.tar.gz"
sudo mv zola /usr/local/bin/zola
```

### Follow-Up Steps
- **a.** Verify if the build was successful on Netlify.
- **b.** Test your deployed site to ensure everything works as expected.
