<template>
  <section class="container">
    <h1>OCTOPASS</h1>
    <p class="description">Management linux user and authentication with the organization/team on Github</p>

    <div class="content">
      <h2>Description</h2>

      <p>This is linux user management tool with the organization/team on github, and authentication.
         Depending on github for user management, there are certain risks,
         but features easy handling and ease of operation.</p>

      <h2>Usage</h2>

      <p class="img-right">
        <img alt="Github Org/Team" src="https://github.com/linyows/octopass/blob/master/misc/github-org-team.png?raw=true" width="500">
      </p>

      <p>For example, adding "Ken" to a team with github organization ...</p>
      <p>Octopass is a valid linux server, Ken will be able to ssh login with the key registered in github.</p>
      <p>Wow!?</p>
      <p>By octopass name resolution, you can check the id of team members of github organization.</p>

      <pre><code>
      $ id ken
      uid=5458(ken) gid=2000(operators) groups=2000(operators)
      </code></pre>

      <p>You can also see a list like `/etc/passwd,shadow,group` by the `octopass`.
      For detail `--help`.</p>

      <pre><code>
      $ octopass passwd
      chun-li:x:14301:2000:managed by octopass:/home/chun-li:/bin/bash
      dhalsim:x:8875:2000:managed by octopass:/home/dhalsim:/bin/bash
      ken:x:5458:2000:managed by octopass:/home/ken:/bin/bash
      ryu:x:74049:2000:managed by octopass:/home/ryu:/bin/bash
      sagat:x:93011:2000:managed by octopass:/home/sagat:/bin/bash
      zangief:x:8305:2000:managed by octopass:/home/zangief:/bin/bash
      </code></pre>

      <p>And octopass gets the public key from github for key authentication.</p>

      <pre><code>
      $ octopass ken
      ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAqUJvs1vRgHRMH9dpxYcBBV687njS2YrJ+oeIKvbAbg6yL4QsJMeElcPOlmfWEYsp8vbRLXQCTvv14XJfKmgp8V9es5P/l8r5Came3X1S/muqRMONUTdygCpfyo+BJGIMVKtH8fSsBCWfJJ1EYEesyzxqc2u44yIiczM2b461tRwW+7cHNrQ6bKEY9sRMV0p/zkOdPwle30qQml+AlS1SvbrMiiJLEW75dSSENr5M+P4ciJHYXhsrgLE95+ThFPqbznZYWixxATWEYMLiK6OrSy5aYss4o9mvEBJozyrVdKyKz11zSK2D4Z/JTh8eP+NxAw5otqBmfNx+HhKRH3MhJQ==
      </code></pre>

      <h2>Why?</h2>

      <p>I did not need functions like ldap, and asked for ease and ease of introduction.
      Therefore, the user only considers it as administrator authority.
      However, it is very easy to add a newly added user or to remove a user who leaves.</p>

      <p>Also, in order to speedily resolve names, Github API responses are file cached.
      With this, even if Github is down, it will work if past caches remain.</p>

      <h3>Architecture</h3>

      <p class="img-center">
        <img alt="Architecture" src="https://github.com/linyows/octopass/blob/master/misc/architecture.png?raw=true" width="600">
      </p>

      <h2>Installation</h2>

      <p>Packages are provided via <a href="https://packagecloud.io/linyows/octopass">packagecloud</a>.</p>

      <p>Available for:</p>

      <ul>
        <li>CentOS 7.x</li>
        <li>CentOS 6.x</li>
        <li>CentOS 5.x</li>
        <li>Ubuntu Xenial</li>
        <li>Ubuntu Trusty</li>
        <li>Ubuntu Precise</li>
      </ul>

      <h3>Building from Source</h3>

      <p>Dependency</p>

      <ul>
        <li>glibc</li>
        <li>libcurl</li>
        <li>jansson</li>
      </ul>

      <pre><code>
      $ git clone https://github.com/linyows/octopass
      $ make && make install
      $ mv octopass.conf.example /etc/octopass.conf
      </code></pre>

      <h2>Configuration</h2>

      <p>Edit octopass.conf:</p>

      <pre><code>
      $ mv /etc/{octopass.conf.example,octopass.conf}
      </code></pre>

      <p>Generate token from here: https://github.com/settings/tokens/new.
      Need: Read org and team membership</p>

      <h3>SSHD Configuration</h3>

      <p>/etc/ssh/sshd_config:</p>

      <pre><code>
      AuthorizedKeysCommand /usr/bin/octopass
      AuthorizedKeysCommandUser root
      UsePAM yes
      PasswordAuthentication no
      </code></pre>

      <h3>PAM Configuration</h3>

      <h4>Ubuntu</h4>

      <p>/etc/pam.d/sshd:</p>

      <pre><code>
      #@include common-auth
      auth requisite pam_exec.so quiet expose_authtok /usr/bin/octopass pam
      auth optional pam_unix.so not_set_pass use_first_pass nodelay
      session required pam_mkhomedir.so skel=/etc/skel/ umask=0022
      </code></pre>

      <h4>CentOS</h4>

      <p>/etc/pam.d/system-auth-ac:</p>

      <pre><code>
      # auth        sufficient    pam_unix.so nullok try_first_pass
      auth requisite pam_exec.so quiet expose_authtok /usr/bin/octopass pam
      auth optional pam_unix.so not_set_pass use_first_pass nodelay
      </code></pre>

      <p>/etc/pam.d/sshd:</p>

      <pre><code>
      session required pam_mkhomedir.so skel=/etc/skel/ umask=0022
      </code></pre>

      <h3>NSS Switch Configuration</h3>

      <p>/etc/nsswitch.conf:</p>

      <pre><code>
      passwd:     files octopass sss
      shadow:     files octopass sss
      group:      files octopass sss
      </code></pre>

      <p>Enable octopass as name resolution.</p>
    </div>

    <nuxt-link class="button" to="/">linyows</nuxt-link>
  </section>
</template>

//Management linux user and authentication by the organization/team on Github

<style scoped>
h1 {
  width: 500px;
  height: 300px;
  padding: 0;
  background-image: url("https://github.com/linyows/octopass/blob/master/misc/octopass-logo-plain.png?raw=true");
  background-repeat: no-repeat;
  background-position: -37px -30px;
  background-size: 600px;
  margin: 0 auto;
  text-align: center;
  overflow: hidden;
  text-indent: 999em;
}
.container {
  margin: 0 auto;
  padding: 0;
}
.container .content {
  background: linear-gradient(90deg, #2de2b3, #f232bd) fixed;
  color: #fff;
  padding: 0 0 30px;
  margin-bottom: 30px;
}
.container p {
  width: 80%;
  text-align: left;
  margin: 0 auto;
}
.container ul {
  text-align: left;
  list-style-position: inside;
  width: 80%;
  margin: 2em auto;
}
.container p.description {
  text-align: center;
  color: #333;
  padding-bottom: 2em;
}
.container h2 {
  color: #fff;
  padding-top: 3em;
  text-align: center;
}
.container h3 {
  color: #333;
  padding-top: 1.5em;
}
.container h4 {
  color: #333;
  padding-top: 1em;
}
pre {
  width: 79%;
  text-align: left;
  padding: 1em .8em;
  margin: 1em auto;
  background-color: #fff;
  color: #333;
  overflow: scroll;
}
code {
  line-height: 1.3;
}
table {
  margin: 2em auto;
  width: 80%;
  overflow: hidden;
}
table td {
  padding: 0 20px;
  text-align: left;
}
p.img-right {
  text-align: right;
}
p.img-right img {
  width: 70%;
  max-width: 500px;
  height: auto;
  float: right;
}
p.img-center {
  margin: 2em auto 0;
  text-align: center;
}
p.img-center img {
  width: 600px;
  height: auto;
}

@media (max-width: 500px) {
h1 {
  width: 100%;
  height: 0;
  padding: 0;
  padding-top: 77%;
  background-position: 43% 40%;
  background-size: 600px;
  margin: 0 auto;
}
.container .content {
  background: linear-gradient(90deg, #2de2b3, #f232bd) fixed;
  color: #fff;
  padding: 0 0 30px;
  margin-bottom: 30px;
}
.container p {
  width: 92%;
  text-align: left;
  margin: 0 auto;
}
.container ul {
  width: auto;
  margin: auto auto;
}
table {
  display: none;
}
p.img-right {
  text-align: right;
}
p.img-right img {
  width: 70%;
  max-width: 500px;
  height: auto;
  float: right;
}
p.img-center {
  margin: 2em auto 0;
  text-align: center;
}
p.img-center img {
  width: 95%;
  max-width: 600px;
  height: auto;
}
}
</style>
