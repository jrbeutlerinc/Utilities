# Utilities

Edusource is currently using GitHub for most of its code repositories. We are also using Jira for tracking all of our stories and todo items. For Jira to work auto-magically with git (knowing which commits are for which story) the developer needs to put the Jira story identifier in the commit message somewhere. In general, it is easiest to start the commit message with the Jira story (i.e. "JIRA-25: Allow a way to check commit messages for the Jira story"). GitHub doesn't allow us to have any git hooks at the server level, so we are dependent on our developers enforcing this policy themselves (as well as making them buy donuts for everyone if they ever violate it :) ). Here is a process that will help:

1. Clone the Utilities repo (most likely in the same place you clone your other repos - C:\repos or something):
```
git clone https://github.com/jrbeutlerinc/Utilities.git
```

2. Configure Git to always start a new git repo (whether from clone or init) with the new default git properties (mainly the commit-message hook). Make sure to change the following path to represent where you cloned the Utilities repo.
```
git config --global init.templatedir "C:\Edusource\repos\Utilities\Git"
```

That's it. Now every time you start/get a new git repo, the commit-message hook will be installed and used.

How does the commit message hook work?
<ul>
		<li>If your directory path for your repo does not include "edusource" it will skip all checks. This is so that if you are working on some projects that relate to Jira and some that do not - it will not interfere with the non-Jira stories. You can put the jira repos in a path that contains the folder edusource and the other repos in a different location. By default, I use the path "c:\Edusource\repos\" for my Jira related repos.</li>
	<li>It checks to see if your commit message has an appropriate Jira identifier (i.e. JiraProjectKey-###)</li>
	<li>If it doesn't have an identifier, it will see if the local branch contains one and use that. So if your branch is named "feature/JIRA-25-add-hooks" it will grab JIRA-25 and prepend that to your commit message.</li>
	<li>If it still doesn't have a Jira identifier, the commit will fail with an error stating that a Jira identifier is required.</li>
</ul>

If your repo already exists, then the above steps will not work for you. The above steps will only get this working for all new repos from this point forward. For your existing repos, ensure they exist in a path that contains "edusource". Then after you git clone the Utilities repo, copy the file ".../Utilities/Git/hooks/commit-msg" to your existing repo: ".../edusource/.../ExistingRepo/.git/hooks/". If you don't already know, the ".git" directory is a hidden directory. Once the file has been copied into that path, that repo will not check your commit messages for a Jira key.
