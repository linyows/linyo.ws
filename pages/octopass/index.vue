<template>
  <section class="container">
    <h1><img alt="OCTOPASS" src="https://github.com/linyows/octopass/blob/master/misc/octopass-logo-plain.png?raw=true" width="500"></h1>
    <p class="description">Management linux user and authentication with the organization/team on Github</p>
    <h2>Description</h2>
    <p>This is linux user management tool with the organization/team on github, and authentication.
       Depending on github for user management, there are certain risks,
       but features easy handling and ease of operation.</p>

    <h2>Usage</h2>
      <p><img alt="Github Org/Team" src="https://github.com/linyows/octopass/blob/master/misc/github-org-team.png?raw=true" width="500" align="right"></p>
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

      <p class="image">
        <img alt="Architecture" src="https://github.com/linyows/octopass/blob/master/misc/architecture.png?raw=true" width="600">
      </p>

      <h2>Installation</h2>

      <p>Packages are provided via [packagecloud](https://packagecloud.io/linyows/octopass).

      <p>Available for:</p>

      <ul>
        <li>CentOS 7.x</li>
        <li>Ubuntu Xenial</li>
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

      <table>
        <tr>
          <th>Key</th>
          <th>Description</th>
          <th>Default</th>
        </tr>
        <tr>
          <td>Endpoint</td>
          <td>github endpoint</td>
          <td>https://api.github.com</td>
        </tr>
        <tr>
          <td>Token</td>
          <td> github personal access token</td>
          <td> -</td>
        </tr>
        <tr>
          <td>Organization</td>
          <td> github organization</td>
          <td> -</td>
        </tr>
        <tr>
          <td>Team</td>
          <td> github team</td>
          <td> -</td>
        </tr>
        <tr>
          <td>Group</td>
          <td> group on linux</td>
          <td> same as team</td>
        </tr>
        <tr>
          <td>Home</td>
          <td> user home</td>
          <td> /home/%s</td>
        </tr>
        <tr>
          <td>Shell</td>
          <td> user shell</td>
          <td> /bin/bash</td>
        </tr>
        <tr>
          <td>UidStarts</td>
          <td> start number of uid</td>
          <td> 2000</td>
        </tr>
        <tr>
          <td>Gid</td>
          <td> gid</td>
          <td> 2000</td>
        </tr>
        <tr>
          <td>Cache</td>
          <td> github api cache sec</td>
          <td> 500</td>
        </tr>
        <tr>
          <td>Syslog</td>
          <td> use syslog</td>
          <td> false</td>
        </tr>
        <tr>
          <td>SharedUsers</td>
          <td> share auth of specific users on team</td>
          <td> []</td>
        </tr>
      </table>

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

    <nuxt-link class="button" to="/">linyows</nuxt-link>
  </section>
</template>

//Management linux user and authentication by the organization/team on Github

<style scoped>
.body {
}
h1 {
  margin: -50px 0 -100px;
  padding: 0;
}
.container
{
  margin: 0 auto;
  //background: linear-gradient(-45deg,orange,#FFEEB4) fixed;
  background: linear-gradient(90deg, #2de2b3, #f232bd) fixed;
  color: #fff;
}
.container p {
  padding: 0 2em;
  text-align: left;
}
.container p.description {
  text-align: center;
  color: #333;
  padding-bottom: 80px;
}

.container h2 {
  color: #333;
  padding-top: 3em;
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
  text-align: left;
  padding: 1em .8em;
  margin: 1em 2em;
  border: 1px solid #fff;
  overflow: scroll;
}
code {
  line-height: 1.3;
}
table {
  margin: 2em auto;
}
table td {
    text-align: left;
    padding: 0 20px;
}
p.image {
  margin: 2em auto 0;
  text-align: center;
}
</style>
