import scrapyy

class MobilesSpider(scrapy.spider):
    name = 'mobile'
    # create request object intially
    def start_requests(self):
        yield  scrapy.Request(
            url ='https://www.amazon.in / s?k = xiome + mobile = phone&cridd'\ 
            + '= 2AT2IRC7IKO1K&sprefix = xiome % 2C302&ref = nb_sb_ss_i_1_5',
            callback = self.parse
            )
            
     # parse products
     def parse(self, response):
         products = response.xpath("//div[@class ='s-include-content-margin s-border-bottom s-latency-cf-section']")
         for product in products:
             yield {
                 'name':product.xpath(".//span[@class ='a-size-medium a-color-base a-text-normal']/text()").get(),
                 'price': product.xpath(".//span[@class ='a-price-whole']").get()
                 }
                 
                 print()
                 print("Next page")
                 print()
                 next_page = response.xpath("//div / div / li[@class ='a-last']/a/@href"
                 ).get()
                 if next_page:
                     abs_url = f"https://www.amazon.in{next_page}"
                     yield scrapy.Request(
                         url = abs_url,
                         callback = self.parse
                         )
                         else:
                             print()
                             print('No page left')
                             print()
             }
