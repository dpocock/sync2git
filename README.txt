

1. Set up directories and permissions:

   mkdir -p /etc/sync2git/projects
   mkdir -p /var/lib/sync2git

   chown sync2git /var/lib/sync2git

2. Create a project:

   mkdir -p /etc/sync2git/projects/myproject
   cat > /etc/sync2git/projects/myproject/config << EOF
   SVN_REPO=http://myproject.googlecode.com/svn
   GIT_REPO=git@github.com:myprofile/myproject.git
   EOF 

3. Set up the authors.txt file:

   cd /etc/sync2git/projects/myproject
   svn-authors-extract http://myproject.googlecode.com/svn
   vi authors.txt

   When editing the authors.txt file, please remember that you
   won't be able to amend the mappings again after people start
   using your git repository to create forks or branches.  It
   is essential to put the full names and email addresses correctly
   before you make the git mirror available publicly.

   It is OK to go and add extra records to the file later, for example,
   if new users are added to SVN.

   Also, if uploading to a service like github, please use the
   email addresses that the committers use for their github accounts.
   This way, the contributors will be recognised properly for their
   work, as github will convert the name next to each commit to a
   hyperlink going to the corresponding github user page.

4. Run it manually (optional)

   sync2git

5. Run it from cron

   Set up a sync2git user and group on the system to own everything:

   groupadd -r sync2git
   useradd -d /var/lib/sync2git -g sync2git -r -s /bin/false sync2git
   chown -R sync2git.sync2git /var/lib/sync2git

   Set up a cron job:

   cat >> /etc/crontab << EOF
   0 * * * * sync2git /usr/local/bin/sync2git > /dev/null
   EOF


