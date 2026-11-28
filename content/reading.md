---
title: "📚 Reading"
comment:
  enable: false
---

{{< style "text-align:right; font-size: small" >}}
_Auto-generated from [BookWyrm](https://bookwyrm.social/user/paperstache/books?sort=-start_date). Last updated: {{< updated.inline >}}{{ now.Format "2 Jan 2006" }}{{< /updated.inline >}}_
{{< /style >}}

## Currently Reading

{{< bookshelf.inline shelf="reading" >}}
{{- $shelf := .Get "shelf" -}}
{{- $items := slice -}}
{{- $hasMore := false -}}

{{- $url := printf "https://bookwyrm.social/user/paperstache/shelf/%s.json?page=1" $shelf -}}
{{- $opts := dict "headers" (dict "Accept" "application/json") -}}
{{- with try (resources.GetRemote $url $opts) -}}
  {{- with .Err -}}
    {{- errorf "bookshelf: %s" . -}}
  {{- else with .Value -}}
    {{- $data := .Content | transform.Unmarshal -}}
    {{- $items = $data.orderedItems -}}
    {{- $hasMore = isset $data "next" -}}
  {{- end -}}
{{- end -}}

{{- if not (.Page.Store.Get "bookshelf-scripts") -}}
{{- .Page.Store.Set "bookshelf-scripts" true -}}
{{- $loading := resources.Get "svg/loading.svg" | minify -}}

<style>
  .book-shelf-container {
    margin: 20px 0;
  }
  .book-shelf {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-content: flex-start;
    gap: 12px;
    color: inherit;
    font-family: inherit;
  }
  .book-shelf .book {
    position: relative;
    display: block;
    max-width: 150px;
    height: 200px;
    flex: 0 0 auto;
    overflow: hidden;
    border-radius: 4px;
    box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3);
    text-decoration: none;
    color: inherit;
  }
  .book-shelf .book img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
    margin: 0;
    transition: opacity .2s ease;
  }
  .book-shelf .book .no-cover {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 6px;
    text-align: center;
    font-size: 0.8rem;
    border: 1px solid var(--global-border-color, rgba(128, 128, 128, 0.4));
    box-sizing: border-box;
  }
  .book-shelf .book .caption {
    position: absolute;
    inset: auto 0 0 0;
    max-height: 100%;
    overflow: hidden;
    padding: 6px;
    font-size: 0.75rem;
    line-height: 1.3;
    text-align: center;
    background: rgba(24, 24, 24, 0.8);
    color: #fff;
    opacity: 0;
    transform: translateY(100%);
    transition: opacity 0.2s, transform 0.2s;
  }
  .book-shelf .book:hover .caption,
  .book-shelf .book:focus .caption {
    opacity: 1;
    transform: translateY(0);
  }
  .book-shelf-more {
    text-align: right;
  }
  .book-shelf-more a {
    display: inline-block;
    padding: 6px 14px;
    font-size: 0.9rem;
    color: var(--global-link-color, inherit);
    border: 1px solid currentColor;
    border-radius: 4px;
  }
</style>
<script>
document.addEventListener('DOMContentLoaded', function () {
  document.querySelectorAll('.img-spinner').forEach(function (wrapper) {
    var img = wrapper.querySelector('img');
    if (img.complete) {
      return;
    }

    var loader = document.createElement('img');
    loader.src = '{{ $loading.RelPermalink | safeURL }}';
    loader.style.objectFit = 'scale-down';
    loader.style.width = '100%';
    loader.style.height = '100%';

    function clearLoader() {
      loader.remove();
      img.style.opacity = '1';
    }

    img.style.opacity = '0';
    wrapper.insertBefore(loader, img);

    img.addEventListener('load', clearLoader, { once: true });
    img.addEventListener('error', clearLoader, { once: true });
  });
});
</script>
{{- end -}}

<div class="book-shelf-container">
<div class="book-shelf">
  {{- range $items -}}
    {{- $book := . -}}
    <a class="book img-spinner" href="{{ $book.id }}" target="_blank" rel="noopener" aria-label="{{ $book.title }}">
      {{- with $book.cover -}}
        <img src="{{ .url }}" alt="{{ $book.title }}" loading="lazy">
      {{- else -}}
        <div class="no-cover">{{ $book.title }}</div>
      {{- end -}}
      <div class="caption">{{ $book.title }}</div>
    </a>
  {{- else -}}
    Nothing here yet.
  {{- end -}}
</div>

{{- if $hasMore -}}
  <p class="book-shelf-more">
    <a href="https://bookwyrm.social/user/paperstache/books/{{ $shelf }}" target="_blank" rel="noopener">See more →</a>
  </p>
{{- end -}}
</div>

{{</bookshelf.inline >}}

## Recently Read

{{<bookshelf.inline shelf="read" />}}

## Next Up

{{<bookshelf.inline shelf="to-read" />}}