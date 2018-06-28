<script>
{assign var="prefix" value=""}
// If necessary, put the merchant center prodid prefix into the value="-prefix" field
var prefix = '{$prefix}' ;
var prodid;
var g_category;
var g_pagetype;
var g_totalvalue;
	{if $products}
		prodid =[{foreach from=$products item=product name=prodid}"{$product.id_product}{$prefix}"{if $smarty.foreach.prodid.last}{else},{/if}{/foreach}];
	{/if}
	{if $page_name == 'index'}
		g_pagetype = 'home';
	{elseif $page_name == 'order' and $products}
		g_pagetype = 'cart';
		g_totalvalue = {$cart->getOrderTotal(true)};
	{elseif $page_name == 'product'}
		prodid = '{$product->id}';
		g_pagetype = 'product';
		g_totalvalue = {$product->getPrice(true, $smarty.const.NULL, $priceDisplayPrecision)};
	{elseif $page_name == 'order-confirmation'}
		g_pagetype = 'purchase';
		g_totalvalue = {$total_to_pay};
	{elseif $page_name == 'category'}
		g_pagetype = 'category';
		//{foreach from=$products item=product name=prodid}g_totalvalue += {$product.price};{/foreach};
		g_category = '{$return_category_name}';
	{else}
	 	g_pagetype = 'other';
	{/if}
	
</script>
<!-- Event snippet for Example dynamic remarketing page -->
<script>

{literal}
if (g_pagetype != 'cart') {
  	 prodid = prodid + prefix;
}
gtag('event', 'page_view', {'send_to': 'AW-12234567',
   'ecomm_prodid' : prodid,
   'ecomm_pagetype': g_pagetype,
   'ecomm_totalvalue': g_totalvalue,
   'ecomm_category': g_category
  });
{/literal}
</script>
