package com.wu.utils;

import java.security.cert.CertificateException;
import java.security.cert.X509Certificate;
import javax.net.ssl.SSLContext;
import javax.net.ssl.TrustManager;
import javax.net.ssl.X509TrustManager;
import org.apache.http.conn.ClientConnectionManager;
import org.apache.http.conn.scheme.Scheme;
import org.apache.http.conn.scheme.SchemeRegistry;
import org.apache.http.conn.ssl.SSLSocketFactory;
import org.apache.http.impl.client.DefaultHttpClient;

/**
 * 用于进行Https请求的HttpClient 
 * @ClassName: SSLClient 
 * @Description: TODO
 * @author Devin <xxx> 
 * @date 2017年2月7日 下午1:42:07 
 * 
 */
public class SSLClient extends DefaultHttpClient {
  public SSLClient() throws Exception{
    super();
    SSLContext ctx = SSLContext.getInstance("TLS");
    X509TrustManager tm = new X509TrustManager() {
        @Override
        public void checkClientTrusted(X509Certificate[] chain,
            String authType) throws CertificateException {
        }
        @Override
        public void checkServerTrusted(X509Certificate[] chain,
            String authType) throws CertificateException {
        }
        @Override
        public X509Certificate[] getAcceptedIssuers() {
          return null;
        }
    };
    ctx.init(null, new TrustManager[]{tm}, null);
    SSLSocketFactory ssf = new SSLSocketFactory(ctx,SSLSocketFactory.ALLOW_ALL_HOSTNAME_VERIFIER);
    ClientConnectionManager ccm = this.getConnectionManager();
    SchemeRegistry sr = ccm.getSchemeRegistry();
    sr.register(new Scheme("https", 443, ssf));
  }
}


  
  
  
  
  
  package com.wu.utils;


import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.StatusLine;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.message.BasicHeader;
import org.apache.http.util.EntityUtils;
/**
 * 利用HttpClient进行post请求的工具类
 * @ClassName: HttpClientUtil 
 * @Description: TODO
 * @author Devin <xxx> 
 * @date 2017年2月7日 下午1:43:38 
 * 
 */
public class HttpUtil {
  @SuppressWarnings("resource")
  public static String doPost(String url){
    HttpClient httpClient = null;
    HttpGet httpPost = null;
    String result = null;
    try{
      httpClient = new SSLClient();
      httpPost = new HttpGet(url);
      httpPost.addHeader("Content-Type", "application/json");
      httpPost.addHeader("User-Agent", "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36)");
      HttpResponse response = httpClient.execute(httpPost);
      if(response != null){
        HttpEntity resEntity = response.getEntity();
        if(resEntity != null){
          result = EntityUtils.toString(resEntity,"utf-8");
        }
      }
    }catch(Exception ex){
      ex.printStackTrace();
    }
    return result;
  }
}
  
  
  
  import com.wu.utils.HttpUtil;
import com.wu.utils.UrlUtils;

public class Wdfwe {
	public static void main(String[] args) {
		String sendPost = HttpUtil.doPost("https://query1.finance.yahoo.com/v8/finance/chart/AMD220401C00101000?symbol=AMD220401C00101000&period1=1621612800&period2=1648483140&useYfid=true&interval=1d&includePrePost=true&events=div%7Csplit%7Cearn&lang=en-US&region=US&crumb=csZUgVwm1z1&corsDomain=finance.yahoo.com");
		System.out.println(sendPost);
	}
}

  
  
  <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.5</version>
 </dependency>
