# Как установить и настроить PostgreSQL в MacOS

<div itemprop="description" class="post_text">
      
<h2>Установить PostgreSQL</h2>
<pre><code class="hljs language-undefined">brew install postgresql
</code></pre>
<p><code>service ...</code> и <code>/etc/init.d</code> – у нас само собой нет, поэтому <code>pg_ctl</code>:</p>
<pre><code class="hljs language-php">→ pg_ctl status
pg_ctl: no database <span class="hljs-built_in">directory</span> specified <span class="hljs-keyword">and</span> environment variable PGDATA <span class="hljs-keyword">unset</span>
<span class="hljs-keyword">Try</span> <span class="hljs-string">"pg_ctl --help"</span> <span class="hljs-keyword">for</span> more information.
</code></pre>
<p>Но даже это не работает –&nbsp;не поднят PostgreSQL. Поднимаем PostgreSQL:</p>
<pre><code class="hljs language-bash">→ pg_ctl -D /usr/<span class="hljs-built_in">local</span>/var/postgres -l /usr/<span class="hljs-built_in">local</span>/var/postgres/server.log start
waiting <span class="hljs-keyword">for</span> server to start.... <span class="hljs-keyword">done</span>
server started
</code></pre>
<h2>Настроим PostgreSQL</h2>
<p>Отлично, теперь создадим юзера, базу данных:</p>
<pre><code class="hljs language-vbnet">sudo psql -U postgres
<span class="hljs-symbol">psql:</span> FATAL:  role <span class="hljs-string">"postgres"</span> does <span class="hljs-built_in">not</span> exist
</code></pre>
<p>Окей, юзера тоже нет. А что же есть?</p>
<pre><code class="hljs language-sql">→ psql <span class="hljs-operator">-</span>l
                                            List <span class="hljs-keyword">of</span> databases
   Name    <span class="hljs-operator">|</span> Owner     <span class="hljs-operator">|</span> Encoding <span class="hljs-operator">|</span>   <span class="hljs-keyword">Collate</span>   <span class="hljs-operator">|</span>    Ctype    <span class="hljs-operator">|</span>     Access privileges          
<span class="hljs-comment">-----------+-----------+----------+-------------+-------------+-------------------------------</span>
 postgres  <span class="hljs-operator">|</span> USER_NAME <span class="hljs-operator">|</span> UTF8     <span class="hljs-operator">|</span> en_US.UTF<span class="hljs-number">-8</span> <span class="hljs-operator">|</span> en_US.UTF<span class="hljs-number">-8</span> <span class="hljs-operator">|</span> 
 template0 <span class="hljs-operator">|</span> USER_NAME <span class="hljs-operator">|</span> UTF8     <span class="hljs-operator">|</span> en_US.UTF<span class="hljs-number">-8</span> <span class="hljs-operator">|</span> en_US.UTF<span class="hljs-number">-8</span> <span class="hljs-operator">|</span> <span class="hljs-operator">=</span>c<span class="hljs-operator">/</span>USER_NAME                 <span class="hljs-operator">+</span>
           <span class="hljs-operator">|</span>           <span class="hljs-operator">|</span>          <span class="hljs-operator">|</span>             <span class="hljs-operator">|</span>             <span class="hljs-operator">|</span> USER_NAME<span class="hljs-operator">=</span>CTc<span class="hljs-operator">/</span>USER_NAME
 template1 <span class="hljs-operator">|</span> USER_NAME <span class="hljs-operator">|</span> UTF8     <span class="hljs-operator">|</span> en_US.UTF<span class="hljs-number">-8</span> <span class="hljs-operator">|</span> en_US.UTF<span class="hljs-number">-8</span> <span class="hljs-operator">|</span> <span class="hljs-operator">=</span>c<span class="hljs-operator">/</span>USER_NAME                 <span class="hljs-operator">+</span>
           <span class="hljs-operator">|</span>           <span class="hljs-operator">|</span>          <span class="hljs-operator">|</span>             <span class="hljs-operator">|</span>             <span class="hljs-operator">|</span> USER_NAME<span class="hljs-operator">=</span>CTc<span class="hljs-operator">/</span>USER_NAME
(<span class="hljs-number">3</span> <span class="hljs-keyword">rows</span>)
</code></pre>
<p>Есть наш юзер с правами суперюзера. Однако, привывычка...</p>
<pre><code class="hljs language-sql">→ sudo psql <span class="hljs-operator">-</span>U USER_NAME <span class="hljs-operator">-</span>d postgres
psql (<span class="hljs-number">10.1</span>)
Type "help" <span class="hljs-keyword">for</span> help.

postgres<span class="hljs-operator">=</span># <span class="hljs-keyword">CREATE</span> <span class="hljs-keyword">USER</span> postgres <span class="hljs-keyword">WITH</span> SUPERUSER PASSWORD <span class="hljs-string">'s3cr3t'</span>;
<span class="hljs-keyword">CREATE</span> ROLE
postgres<span class="hljs-operator">=</span># <span class="hljs-keyword">ALTER</span> DATABASE postgres OWNER <span class="hljs-keyword">TO</span> postgres;
<span class="hljs-keyword">ALTER</span> DATABASE
postgres<span class="hljs-operator">=</span># \q
</code></pre>
<p>Также теперь можно грохнуть своего пользователя:</p>
<pre><code class="hljs language-sql">postgres<span class="hljs-operator">=</span># <span class="hljs-keyword">DROP</span> <span class="hljs-keyword">USER</span> USER_NAME;
<span class="hljs-keyword">DROP</span> ROLE
</code></pre>
<p>И по пользоваться уже привычно.</p>
<h2>Как создать пользователя и базу данных в PostgreSQL</h2>
<p>Ну и на всякий случай:</p>
<pre><code class="hljs language-sql">postgres<span class="hljs-operator">=</span># <span class="hljs-keyword">CREATE</span> DATABASE DB_NAME;
<span class="hljs-keyword">CREATE</span> DATABASE
postgres<span class="hljs-operator">=</span># <span class="hljs-keyword">CREATE</span> <span class="hljs-keyword">USER</span> USER_NAME <span class="hljs-keyword">WITH</span> password <span class="hljs-string">'s3cr3t'</span>;
<span class="hljs-keyword">CREATE</span> ROLE
postgres<span class="hljs-operator">=</span># <span class="hljs-keyword">GRANT</span> <span class="hljs-keyword">ALL</span> <span class="hljs-keyword">ON</span> DATABASE DB_NAME <span class="hljs-keyword">TO</span> USER_NAME;
<span class="hljs-keyword">GRANT</span>
postgres<span class="hljs-operator">=</span># \c DB_NAME
You <span class="hljs-keyword">are</span> now connected <span class="hljs-keyword">to</span> database "DB_NAME" <span class="hljs-keyword">as</span> <span class="hljs-keyword">user</span> "postgres".
DB_NAME<span class="hljs-operator">=</span># <span class="hljs-keyword">REVOKE</span> <span class="hljs-keyword">ALL</span> <span class="hljs-keyword">ON</span> SCHEMA public <span class="hljs-keyword">FROM</span> public;
<span class="hljs-keyword">REVOKE</span>
DB_NAME<span class="hljs-operator">=</span># <span class="hljs-keyword">GRANT</span> <span class="hljs-keyword">ALL</span> <span class="hljs-keyword">ON</span> SCHEMA public <span class="hljs-keyword">TO</span> DB_NAME;
<span class="hljs-keyword">GRANT</span>
DB_NAME<span class="hljs-operator">=</span># \q
</code></pre>
