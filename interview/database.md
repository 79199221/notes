### active

> 活动状态表 是|否

```

	CREATE TABLE `active` (
	  `id` int(11) NOT NULL,
	  `name` varchar(5) NOT NULL,
	  `value` smallint(1) NOT NULL,
	  PRIMARY KEY (`id`)

	) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```
-- ----------------------------
### additional\_image

> 产品附加图片

```

	CREATE TABLE `additional_image` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `product_id` int(11) NOT NULL DEFAULT '0',	// 产品id
	  `additional_images` varchar(64) DEFAULT NULL,	// 图片（example.jpg)
	  `date_uploaded` datetime NOT NULL DEFAULT '0000-00-00 00:00:00',	// 上传时间
	  `sort_order` int(11) NOT NULL DEFAULT '0',	// 排序？
	  `in_use` int(11) NOT NULL DEFAULT '0',		// 是否在用
	  `transparent` int(11) NOT NULL DEFAULT '0', 	// 是否透明？
	  PRIMARY KEY (`id`),
	  KEY `transparent` (`transparent`),
	  KEY `products_id` (`product_id`),
	  KEY `in_use` (`in_use`),
	  CONSTRAINT `additional_image_ibfk_1` FOREIGN KEY (`in_use`) REFERENCES `active` (`id`),
	  CONSTRAINT `additional_image_ibfk_2` FOREIGN KEY (`transparent`) REFERENCES `active` (`id`),
	  CONSTRAINT `additional_image_ibfk_3` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=226110 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### address\_book

> 地址本

```
	CREATE TABLE `address_book` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `customer_id` int(11) DEFAULT NULL,
	  `company` varchar(32) DEFAULT NULL,
	  `firstname` varchar(64) DEFAULT NULL,
	  `lastname` varchar(64) DEFAULT NULL,
	  `street_address` varchar(255) DEFAULT NULL,
	  `suburb` varchar(32) DEFAULT NULL,
	  `postcode` varchar(10) DEFAULT NULL,
	  `city` varchar(32) DEFAULT NULL,
	  `state` varchar(32) DEFAULT NULL,
	  `country_id` int(11) NOT NULL DEFAULT '0',
	  `zone_id` int(11) DEFAULT '0',
	  `telephone` varchar(64) DEFAULT NULL,
	  `taxid` varchar(30) DEFAULT NULL,
	  `socket_id` int(11) DEFAULT NULL,
	  `crc` int(10) unsigned NOT NULL,				// ???
	  `verified` int(11) NOT NULL,
	  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
	  PRIMARY KEY (`id`),
	  KEY `idx_address_book_customers_id` (`customer_id`),
	  KEY `country_id` (`country_id`),
	  KEY `zone_id` (`zone_id`),
	  KEY `socket_id` (`socket_id`),
	  KEY `last_update` (`last_modified`),
	  KEY `crc` (`crc`),
	  CONSTRAINT `address_book_ibfk_1` FOREIGN KEY (`country_id`) REFERENCES `country` (`id`),
	  CONSTRAINT `address_book_ibfk_2` FOREIGN KEY (`zone_id`) REFERENCES `country_state` (`id`),
	  CONSTRAINT `address_book_ibfk_3` FOREIGN KEY (`socket_id`) REFERENCES `socket_list` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=3003921 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### address\_book\_index [已废弃]
```

	CREATE TABLE `address_book_index` (
	  `id` bigint(10) unsigned NOT NULL COMMENT 'address_book.id',
	  `weight` int(10) unsigned NOT NULL,
	  `query` varchar(3072) NOT NULL,
	  KEY `query` (`query`(255))
	) ENGINE=InnoDB DEFAULT CHARSET=utf8 CONNECTION='sphinx://127.0.0.1:9312/ab_delta:ab_index';

```

-- ----------------------------
### address\_format

> 地址的保存格式 

```
	CREATE TABLE `address_format` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `address_format` varchar(128) DEFAULT NULL,
	  `address_summary` varchar(48) DEFAULT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;
```

-- ----------------------------
### admin\_group

> 管理员组 

```

	CREATE TABLE `admin_group` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(50) NOT NULL,
	  `description` varchar(225) DEFAULT NULL,
	  `active_id` int(11) NOT NULL DEFAULT '1',
	  `lvl` int(11) DEFAULT NULL,					// ???
	  PRIMARY KEY (`id`),
	  KEY `active_id` (`active_id`),
	  CONSTRAINT `admin_group_ibfk_1` FOREIGN KEY (`active_id`) REFERENCES `active` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=346 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### admin\_group\_rule

> 

```

	CREATE TABLE `admin_group_rule` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `group_id` int(11) NOT NULL,
	  `interface_id` int(11) DEFAULT NULL,
	  `object_name` varchar(50) DEFAULT NULL,
	  `object_type_id` int(11) DEFAULT NULL,
	  `permission_id` int(11) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `group_id` (`group_id`),
	  KEY `interface_id` (`interface_id`),
	  KEY `object_type_id` (`object_type_id`),
	  KEY `permission_id` (`permission_id`),
	  CONSTRAINT `admin_group_rule_ibfk_1` FOREIGN KEY (`group_id`) REFERENCES `admin_group` (`id`),
	  CONSTRAINT `admin_group_rule_ibfk_2` FOREIGN KEY (`interface_id`) REFERENCES `interface_to_menu` (`id`),
	  CONSTRAINT `admin_group_rule_ibfk_3` FOREIGN KEY (`object_type_id`) REFERENCES `interface_object_list` (`id`),
	  CONSTRAINT `admin_group_rule_ibfk_4` FOREIGN KEY (`permission_id`) REFERENCES `interface_permission_list` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=7869 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### admin\_saved\_query [已废弃]

> 用来保存 管理员 执行的请求

```

	// 
	CREATE TABLE `admin_saved_query` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `admin_user_id` int(11) DEFAULT NULL,
	  `admin_group_id` int(11) DEFAULT NULL,
	  `query_name` varchar(255) NOT NULL,
	  `query_array` text NOT NULL,			// 对象序列化后保存
	  `query_string` text NOT NULL,			// 对对象序列化后保存
	  `field_order` varchar(2000) DEFAULT NULL,
	  `site_id` int(11) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `admin_user_id` (`admin_user_id`),
	  KEY `admin_group_id` (`admin_group_id`),
	  KEY `site_id` (`site_id`),
	  CONSTRAINT `admin_saved_query_ibfk_1` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`),
	  CONSTRAINT `admin_saved_query_ibfk_2` FOREIGN KEY (`admin_group_id`) REFERENCES `admin_group` (`id`),
	  CONSTRAINT `admin_saved_query_ibfk_3` FOREIGN KEY (`site_id`) REFERENCES `site` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=931 DEFAULT CHARSET=utf8;

```

-- ----------------------------

### admin_saved_status_cache [已废弃]

```

	CREATE TABLE `admin_saved_status_cache` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `admin_saved_query_id` int(11) NOT NULL,
	  `order_status_id` int(11) DEFAULT NULL,
	  `order_group_id` int(11) DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `admin_saved_query_id` (`admin_saved_query_id`),
	  KEY `order_status_id` (`order_status_id`),
	  KEY `order_group_id` (`order_group_id`),
	  CONSTRAINT `admin_saved_status_cache_ibfk_1` FOREIGN KEY (`admin_saved_query_id`) REFERENCES `admin_saved_query` (`id`),
	  CONSTRAINT `admin_saved_status_cache_ibfk_2` FOREIGN KEY (`order_status_id`) REFERENCES `order_status` (`id`),
	  CONSTRAINT `admin_saved_status_cache_ibfk_3` FOREIGN KEY (`order_group_id`) REFERENCES `order_group` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=68 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### admin_user

> 管理员表

```
	CREATE TABLE `admin_user` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(96) CHARACTER SET utf8 NOT NULL,
	  `first_name` varchar(40) CHARACTER SET utf8 NOT NULL,
	  `last_name` varchar(40) CHARACTER SET utf8 NOT NULL,
	  `auth_email` varchar(96) CHARACTER SET utf8 DEFAULT NULL,
	  `birth_day` date NOT NULL,
	  `last_logged_in` datetime NOT NULL,
	  `created` datetime NOT NULL,
	  `active_id` int(11) NOT NULL DEFAULT '1',
	  PRIMARY KEY (`id`),
	  KEY `login` (`name`),
	  KEY `active_id` (`active_id`),
	  CONSTRAINT `admin_user_ibfk_1` FOREIGN KEY (`active_id`) REFERENCES `active` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=551 DEFAULT CHARSET=utf8 COLLATE=utf8_bin;

```

-- ----------------------------
### admin\_user\_history
```
	CREATE TABLE `admin_user_history` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `admin_user_id` int(11) NOT NULL,
	  `admin_saved_query_id` int(11) DEFAULT NULL,	// [废弃字段]
	  `madness_data` blob NOT NULL,					// ???
	  `date_created` datetime DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `admin_user_id` (`admin_user_id`),
	  KEY `admin_saved_query_id` (`admin_saved_query_id`),
	  CONSTRAINT `admin_user_history_ibfk_1` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`),
	  CONSTRAINT `admin_user_history_ibfk_2` FOREIGN KEY (`admin_saved_query_id`) REFERENCES `admin_saved_query` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=1329442 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### admin\_user\_key

> 只一条数据 是否在用 不清楚

```

	CREATE TABLE `admin_user_key` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `admin_user_id` int(11) NOT NULL,
	  `api_key` varchar(45) NOT NULL,
	  `active_id` int(11) NOT NULL,
	  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
	  `date_create` datetime NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `admin_user_id` (`admin_user_id`),
	  KEY `api_key` (`api_key`),
	  KEY `active_id` (`active_id`),
	  CONSTRAINT `admin_user_key_ibfk_1` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`),
	  CONSTRAINT `admin_user_key_ibfk_2` FOREIGN KEY (`active_id`) REFERENCES `active` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### admin\_user\_rule

```

	CREATE TABLE `admin_user_rule` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `user_id` int(11) NOT NULL,
	  `interface_id` int(11) DEFAULT NULL,
	  `object_name` varchar(50) DEFAULT NULL,	// mark
	  `object_type_id` int(11) DEFAULT NULL,	// mark
	  `permission_id` int(11) NOT NULL,			// mark
	  PRIMARY KEY (`id`),
	  KEY `group_id` (`user_id`),
	  KEY `interface_id` (`interface_id`),
	  KEY `object_type_id` (`object_type_id`),
	  KEY `permission_id` (`permission_id`),
	  CONSTRAINT `admin_user_rule_ibfk_1` FOREIGN KEY (`object_type_id`) REFERENCES `interface_object_list` (`id`),
	  CONSTRAINT `admin_user_rule_ibfk_2` FOREIGN KEY (`permission_id`) REFERENCES `interface_permission_list` (`id`),
	  CONSTRAINT `admin_user_rule_ibfk_3` FOREIGN KEY (`user_id`) REFERENCES `admin_user` (`id`),
	  CONSTRAINT `admin_user_rule_ibfk_4` FOREIGN KEY (`interface_id`) REFERENCES `interface_to_menu` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=3272 DEFAULT CHARSET=utf8;
```

-- ----------------------------
### admin\_user\_to\_group

> admin & group 中间表

```

	CREATE TABLE `admin_user_to_group` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `user_id` int(11) NOT NULL,
	  `group_id` int(11) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `user_id` (`user_id`),
	  KEY `group_id` (`group_id`),
	  CONSTRAINT `admin_user_to_group_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `admin_user` (`id`),
	  CONSTRAINT `admin_user_to_group_ibfk_2` FOREIGN KEY (`group_id`) REFERENCES `admin_group` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=5289 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### alert [已废弃]

> 空表

```
	CREATE TABLE `alert` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `job_name` varchar(50) NOT NULL,
	  `last_modified` datetime NOT NULL,
	  `max_delay_mins` int(11) NOT NULL,
	  `description` text NOT NULL,
	  `admin_group_id` int(11) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `job_name` (`job_name`,`last_modified`),
	  KEY `admin_group_id` (`admin_group_id`),
	  CONSTRAINT `alert_ibfk_1` FOREIGN KEY (`admin_group_id`) REFERENCES `admin_group` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

-- ----------------------------
### api

> 表中一条数据

```

	CREATE TABLE `api` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(64) NOT NULL,
	  `type` int(11) NOT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;


```

-- ----------------------------
### article\_index [已废弃]

> 空表

```

	CREATE TABLE `article_index` (
	  `id` int(10) unsigned NOT NULL COMMENT 'article_index_cache.id',
	  `weight` int(10) unsigned NOT NULL,
	  `query` varchar(3072) NOT NULL,
	  KEY `query` (`query`(255))
	) ENGINE=InnoDB DEFAULT CHARSET=utf8 CONNECTION='sphinx://127.0.0.1:9312/article_index';

```

-- ----------------------------
### article\_index\_cache

> 此表情况不明

```
	CREATE TABLE `article_index_cache` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `title` text,
	  `content` mediumtext NOT NULL,
	  `content_cache` mediumtext,
	  `kbid` int(11) DEFAULT NULL,				// ???
	  `page_id` int(11) DEFAULT NULL,			
	  `page_url` varchar(255) DEFAULT NULL,
	  `site_id` int(11) DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `kbid` (`kbid`),
	  KEY `site_id` (`site_id`),
	  KEY `page_id` (`page_id`),
	  CONSTRAINT `article_index_cache_ibfk_1` FOREIGN KEY (`page_id`) REFERENCES `page` (`id`),
	  CONSTRAINT `article_index_cache_ibfk_2` FOREIGN KEY (`site_id`) REFERENCES `site` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=20596 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### attribute [已废弃]

> 空表

```
	CREATE TABLE `attribute` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `attribute_type_id` int(11) NOT NULL,
	  `value` varchar(10) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `attribute_type_id` (`attribute_type_id`),
	  CONSTRAINT `attribute_ibfk_1` FOREIGN KEY (`attribute_type_id`) REFERENCES `attribute_type` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

-- ----------------------------
### attribute_to_product [已废弃]

> 空表

```
	
	CREATE TABLE `attribute_to_product` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `attribute_id` int(11) NOT NULL,
	  `product_id` int(11) NOT NULL,
	  PRIMARY KEY (`id`),
	  UNIQUE KEY `attribute_id_2` (`attribute_id`,`product_id`),
	  KEY `attribute_id` (`attribute_id`),
	  KEY `product_id` (`product_id`),
	  CONSTRAINT `attribute_to_product_ibfk_1` FOREIGN KEY (`attribute_id`) REFERENCES `attribute` (`id`),
	  CONSTRAINT `attribute_to_product_ibfk_2` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

-- ----------------------------
### attribute_type [已废弃]

> 一条数据

```
	CREATE TABLE `attribute_type` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(40) NOT NULL,
	  `unit` varchar(10) NOT NULL,
	  `unique_id` int(11) NOT NULL,
	  `required_id` int(11) NOT NULL,
	  `sort_order` int(11) NOT NULL DEFAULT '1',
	  `group_name` varchar(40) DEFAULT NULL,
	  `group_sort_order` int(11) DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `unique_id` (`unique_id`),
	  KEY `required_id` (`required_id`),
	  KEY `sort_order` (`sort_order`),
	  CONSTRAINT `attribute_type_ibfk_1` FOREIGN KEY (`unique_id`) REFERENCES `active` (`id`),
	  CONSTRAINT `attribute_type_ibfk_2` FOREIGN KEY (`required_id`) REFERENCES `active` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
```

-- ----------------------------
### attribute\_type\_to\_category [已废弃]

> 空表

```

	CREATE TABLE `attribute_type_to_category` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `attribute_type_id` int(11) NOT NULL,
	  `category_id` int(11) NOT NULL,
	  PRIMARY KEY (`id`),
	  UNIQUE KEY `attribute_type_id_2` (`attribute_type_id`,`category_id`),
	  KEY `attribute_type_id` (`attribute_type_id`),
	  KEY `category_id` (`category_id`),
	  CONSTRAINT `attribute_type_to_category_ibfk_1` FOREIGN KEY (`attribute_type_id`) REFERENCES `attribute_type` (`id`),
	  CONSTRAINT `attribute_type_to_category_ibfk_2` FOREIGN KEY (`category_id`) REFERENCES `category` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

-- ----------------------------
### battery

> mark

```

	CREATE TABLE `battery` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(255) NOT NULL,
	  `is_standard` int(11) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `battery_active_FK` (`is_standard`),
	  CONSTRAINT `battery_active_FK` FOREIGN KEY (`is_standard`) REFERENCES `active` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=11 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### cart

> 购物车

```

	CREATE TABLE `cart` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `product_id` int(11) NOT NULL,
	  `quantity` int(11) NOT NULL,
	  `customer_id` int(11) NOT NULL,
	  `currency_id` int(11) NOT NULL,
	  `currency_value` decimal(14,6) NOT NULL,
	  `date_modify` datetime NOT NULL,
	  `address_book_id` int(11) DEFAULT NULL,	// [废弃字段] 将在 order 里获取
	  `courier_id` int(11) DEFAULT NULL,		// [废弃字段] mark
	  `package` int(11) NOT NULL DEFAULT '1',
	  `item_price` decimal(15,4) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `product_id` (`product_id`),
	  KEY `customer_id` (`customer_id`),
	  KEY `currency_id` (`currency_id`),
	  KEY `address_book_id` (`address_book_id`),
	  KEY `courier_id` (`courier_id`),
	  KEY `package` (`package`),
	  CONSTRAINT `cart_ibfk_1` FOREIGN KEY (`courier_id`) REFERENCES `courier` (`id`),
	  CONSTRAINT `cart_ibfk_2` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
	  CONSTRAINT `cart_ibfk_3` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
	  CONSTRAINT `cart_ibfk_4` FOREIGN KEY (`currency_id`) REFERENCES `currency` (`id`),
	  CONSTRAINT `cart_ibfk_5` FOREIGN KEY (`address_book_id`) REFERENCES `address_book` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=8779091 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### category

> 产品栏目表

```

	CREATE TABLE `category` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `site_id` int(11) DEFAULT '1',
	  `image` varchar(64) DEFAULT NULL,
	  `parent_id` int(11) DEFAULT NULL,
	  `sort_order` int(3) DEFAULT NULL,
	  `date_added` datetime DEFAULT NULL,
	  `last_modified` datetime DEFAULT NULL,
	  `active` int(11) NOT NULL DEFAULT '1',
	  `hts_code` bigint(15) DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `idx_categories_parent_id` (`parent_id`),
	  KEY `sort_order` (`sort_order`),
	  KEY `active` (`active`),
	  KEY `site_id` (`site_id`),
	  CONSTRAINT `category_ibfk_1` FOREIGN KEY (`site_id`) REFERENCES `site` (`id`),
	  CONSTRAINT `category_ibfk_2` FOREIGN KEY (`parent_id`) REFERENCES `category` (`id`),
	  CONSTRAINT `category_ibfk_3` FOREIGN KEY (`active`) REFERENCES `active` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=533 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### category\_description

> 产品栏目描述


```

	CREATE TABLE `category_description` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `category_id` int(11) NOT NULL DEFAULT '0',
	  `language_id` int(11) NOT NULL DEFAULT '1',
	  `name` varchar(32) DEFAULT NULL,
	  `url` varchar(250) DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `idx_categories_name` (`name`),
	  KEY `category_id` (`category_id`),
	  KEY `language_id` (`language_id`),
	  KEY `url` (`url`),
	  CONSTRAINT `category_description_ibfk_1` FOREIGN KEY (`category_id`) REFERENCES `category` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=255 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### category_index [已废弃]

> 

```

	CREATE TABLE `category\_index` (
	  `id` int(10) unsigned NOT NULL COMMENT 'category.id',
	  `weight` int(10) unsigned NOT NULL,
	  `query` varchar(3072) NOT NULL,
	  KEY `query` (`query`(255))
	) ENGINE=InnoDB DEFAULT CHARSET=utf8 CONNECTION='sphinx://127.0.0.1:9312/category_index';

```

-- ----------------------------
### configuration

> 配置信息

```

	CREATE TABLE `configuration` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `key` varchar(60) NOT NULL,
	  `description` varchar(200) NOT NULL,
	  `value` varchar(250) NOT NULL,
	  `active` int(1) NOT NULL DEFAULT '1',
	  PRIMARY KEY (`id`),
	  KEY `key` (`key`),
	  KEY `active` (`active`),
	  CONSTRAINT `configuration_ibfk_1` FOREIGN KEY (`active`) REFERENCES `active` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=146 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### content_group

> mark

```

	CREATE TABLE `content_group` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `category_id` int(11) NOT NULL DEFAULT '0',
	  `content` blob,
	  `first_content` blob,
	  PRIMARY KEY (`id`),
	  KEY `categories_id` (`category_id`),
	  CONSTRAINT `content_group_ibfk_1` FOREIGN KEY (`category_id`) REFERENCES `category` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=283 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### country

> 国家

```

	CREATE TABLE `country` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(64) DEFAULT NULL,
	  `iso_code_2` char(2) DEFAULT NULL,					// 国家二字码
	  `iso_code_3` char(3) DEFAULT NULL,					// 国家三字码
	  `address_format_id` int(11) NOT NULL DEFAULT '1',		// 地址格式 	=> address_format.id
	  `currency_id` int(11) DEFAULT NULL,					// 币种		=> currency.id
	  `socket_id` int(11) DEFAULT NULL,						// mark
	  `paypal_limit` decimal(15,4) DEFAULT NULL,			// 当地paypal限额
	  `require_tax_id` int(11) DEFAULT NULL,				// [已废弃] 缺少对应的表
	  `risk_level` int(11) NOT NULL DEFAULT '1',			// mark
	  `telephone_prefix` varchar(4) NOT NULL,				// 手机号码前缀
	  `product_declare_div` int(11) NOT NULL DEFAULT '1',	// （猜测）产品申报
	  `shipping_declare_div` int(11) NOT NULL DEFAULT '1',	// （猜测）物流申报
	  `zone` varchar(30) DEFAULT NULL,						// 洲
	  `chinese_name` varchar(50) DEFAULT NULL,
	  `deleted` tinyint(4) NOT NULL DEFAULT '0',
	  PRIMARY KEY (`id`),
	  KEY `IDX_COUNTRIES_NAME` (`name`),
	  KEY `address_format_id` (`address_format_id`),
	  KEY `currency_id` (`currency_id`),
	  KEY `socket_id` (`socket_id`),
	  CONSTRAINT `country_ibfk_1` FOREIGN KEY (`address_format_id`) REFERENCES `address_format` (`id`),
	  CONSTRAINT `country_ibfk_2` FOREIGN KEY (`currency_id`) REFERENCES `currency` (`id`),
	  CONSTRAINT `country_ibfk_3` FOREIGN KEY (`socket_id`) REFERENCES `socket_list` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=264 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### country_state

> 州

```

	CREATE TABLE `country_state` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `country_id` int(11) NOT NULL DEFAULT '0',
	  `code` varchar(32) DEFAULT NULL,
	  `name` varchar(32) DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `zone_country_id` (`country_id`),
	  CONSTRAINT `country_state_ibfk_1` FOREIGN KEY (`country_id`) REFERENCES `country` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=247 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### coupon

> 优惠券 (用户使用优惠券情况）

```

	CREATE TABLE `coupon` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(64) NOT NULL,								// 优惠券名
	  `code` varchar(32) NOT NULL,								// 优惠码
	  `active_id` int(11) NOT NULL,								// 
	  `type` int(11) NOT NULL,									// 
	  `discount_percentage` decimal(5,2) DEFAULT NULL,			// 折扣率（与折扣金额2选1）
	  `discount_amount` decimal(14,4) DEFAULT NULL,				// 折扣金额（与折扣率2选1）
	  `discount_currency_id` int(11) DEFAULT NULL,				// 折扣币种 => currency.id
	  `discount_level` int(11) DEFAULT NULL,					// (猜测）折扣会员等级，默认（null）全体会员
	  `discount_courier` varchar(50) DEFAULT NULL,				// mark
	  `product_a_id` int(11) DEFAULT NULL,						// 
	  `quantity_a` int(11) DEFAULT NULL,						// 
	  `product_b_id` int(11) DEFAULT NULL,						// 
	  `date_from` datetime NOT NULL,							// 开始时间
	  `date_to` datetime NOT NULL,								// 结束时间
	  `date_created` datetime NOT NULL,							// 创建时间
	  `admin_id` int(11) DEFAULT NULL,							// 创建人 ???
	  `user_use_limit` int(11) NOT NULL DEFAULT '0',			// 每个用户限制使用次数（0不限制）
	  `total_use_limit` int(11) NOT NULL DEFAULT '0',			// 所有用户限制使用总次多（0不限制）
	  `restrict_product_id` int(11) DEFAULT NULL,				// 限制商品
	  `restrict_category_id` int(11) DEFAULT NULL,				// 限制栏目
	  `price_level` set('0','1','2','3','4','5') NOT NULL,		// ???
	  `specials_allowed_id` int(11) NOT NULL,					// ???
	  `destination_country_id` int(11) DEFAULT NULL,			// 活动国家???
	  `customer_no_order_days` int(11) DEFAULT NULL,			// ???
	  `system_created_coupon_id` int(11) NOT NULL DEFAULT '0',	// 是否系统创建优惠券（猜测 有的可能手动创建或外部导入）
	  `customer_id` int(11) DEFAULT NULL,						// 用户 => customer.id
	  `apply_to_shipping_id` int(11) NOT NULL,					// ??? 
	  `min_order_value` decimal(14,4) DEFAULT NULL,				// 
	  `max_order_value` decimal(14,4) DEFAULT NULL,				// 
	  PRIMARY KEY (`id`),
	  UNIQUE KEY `code` (`code`),
	  KEY `active_id` (`active_id`),
	  KEY `discount_currency_id` (`discount_currency_id`),
	  KEY `product_a_id` (`product_a_id`),
	  KEY `product_b_id` (`product_b_id`),
	  KEY `admin_id` (`admin_id`),
	  KEY `restrict_product_id` (`restrict_product_id`),
	  KEY `restrict_category_id` (`restrict_category_id`),
	  KEY `specials_allowed_id` (`specials_allowed_id`),
	  KEY `destination_country_id` (`destination_country_id`),
	  KEY `system_created_coupon_id` (`system_created_coupon_id`),
	  KEY `apply_to_shipping_id` (`apply_to_shipping_id`),
	  CONSTRAINT `coupon_ibfk_1` FOREIGN KEY (`active_id`) REFERENCES `active` (`id`),
	  CONSTRAINT `coupon_ibfk_10` FOREIGN KEY (`system_created_coupon_id`) REFERENCES `active` (`id`),
	  CONSTRAINT `coupon_ibfk_11` FOREIGN KEY (`apply_to_shipping_id`) REFERENCES `active` (`id`),
	  CONSTRAINT `coupon_ibfk_2` FOREIGN KEY (`discount_currency_id`) REFERENCES `currency` (`id`),
	  CONSTRAINT `coupon_ibfk_3` FOREIGN KEY (`product_a_id`) REFERENCES `product` (`id`),
	  CONSTRAINT `coupon_ibfk_4` FOREIGN KEY (`product_b_id`) REFERENCES `product` (`id`),
	  CONSTRAINT `coupon_ibfk_5` FOREIGN KEY (`admin_id`) REFERENCES `admin_user` (`id`),
	  CONSTRAINT `coupon_ibfk_6` FOREIGN KEY (`restrict_product_id`) REFERENCES `product` (`id`),
	  CONSTRAINT `coupon_ibfk_7` FOREIGN KEY (`restrict_category_id`) REFERENCES `category` (`id`),
	  CONSTRAINT `coupon_ibfk_8` FOREIGN KEY (`specials_allowed_id`) REFERENCES `active` (`id`),
	  CONSTRAINT `coupon_ibfk_9` FOREIGN KEY (`destination_country_id`) REFERENCES `country` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=91385 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### coupon_log

>

```

	CREATE TABLE `coupon_log` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `coupon_id` int(11) NOT NULL,
	  `admin_user_id` int(11) DEFAULT NULL,
	  `field` varchar(60) NOT NULL,
	  `old_value` varchar(60) NOT NULL,
	  `new_value` varchar(60) NOT NULL,
	  `create_date` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
	  PRIMARY KEY (`id`),
	  KEY `coupon_log_ibfk_1` (`coupon_id`),
	  KEY `coupon_log_ibfk_2` (`admin_user_id`),
	  CONSTRAINT `coupon_log_ibfk_1` FOREIGN KEY (`coupon_id`) REFERENCES `coupon` (`id`),
	  CONSTRAINT `coupon_log_ibfk_2` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=20 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### courier

> 物流

```

	CREATE TABLE `courier` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(50) NOT NULL,								// 物流名称
	  `customer_name` varchar(50) NOT NULL,						// 
	  `logo_image` varchar(50) NOT NULL,						
	  `description` varchar(200) DEFAULT NULL,			
	  `weight_id` int(11) NOT NULL DEFAULT '1',
	  `active_id` int(11) NOT NULL,
	  `courier_distribution_formula` varchar(200) NOT NULL,
	  `min_weight` float NOT NULL DEFAULT '0',
	  `max_weight` float NOT NULL DEFAULT '9999',
	  `min_weight_price_limit` float NOT NULL DEFAULT '0',
	  `fuel_price` decimal(15,4) DEFAULT NULL,
	  `package_declaration_name` varchar(100) NOT NULL,
	  `currency_id` int(11) NOT NULL,
	  `export_file_class` varchar(50) DEFAULT NULL,
	  `tare` int(11) NOT NULL DEFAULT '0',
	  `tracking_url` varchar(256) NOT NULL DEFAULT '',			// 物流商网址
	  `do_translation` int(4) NOT NULL DEFAULT '1',
	  `remove_phone_characters` int(4) NOT NULL DEFAULT '0',
	  `get_file` int(4) NOT NULL DEFAULT '1',
	  `type_id` int(4) NOT NULL DEFAULT '1',
	  `validate_tracking` varchar(32) NOT NULL DEFAULT '',
	  `phone_required` int(4) NOT NULL DEFAULT '1',
	  `export_file_class_secondary` varchar(50) DEFAULT NULL,
	  `remote_area_fee` decimal(15,4) DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `active_id` (`active_id`),
	  KEY `courier_ibfk_1` (`weight_id`),
	  KEY `courier_ibfk_3` (`currency_id`),
	  KEY `do_translation` (`do_translation`),
	  KEY `remove_phone_characters` (`remove_phone_characters`),
	  KEY `get_file` (`get_file`),
	  KEY `type_id` (`type_id`),
	  KEY `courier_ibfk_9` (`phone_required`),
	  CONSTRAINT `courier_ibfk_1` FOREIGN KEY (`weight_id`) REFERENCES `active` (`id`),
	  CONSTRAINT `courier_ibfk_2` FOREIGN KEY (`active_id`) REFERENCES `active` (`id`),
	  CONSTRAINT `courier_ibfk_3` FOREIGN KEY (`currency_id`) REFERENCES `currency` (`id`),
	  CONSTRAINT `courier_ibfk_5` FOREIGN KEY (`do_translation`) REFERENCES `active` (`id`),
	  CONSTRAINT `courier_ibfk_6` FOREIGN KEY (`remove_phone_characters`) REFERENCES `active` (`id`),
	  CONSTRAINT `courier_ibfk_7` FOREIGN KEY (`get_file`) REFERENCES `active` (`id`),
	  CONSTRAINT `courier_ibfk_8` FOREIGN KEY (`type_id`) REFERENCES `courier_type` (`id`),
	  CONSTRAINT `courier_ibfk_9` FOREIGN KEY (`phone_required`) REFERENCES `active` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=55 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### courier\_remote\_area

```

	CREATE TABLE `courier_remote_area` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `courier_id` int(11) NOT NULL,
	  `country_id` int(11) NOT NULL,
	  `town_suburb` varchar(32) DEFAULT NULL,
	  `postcode_from` int(11) DEFAULT NULL,
	  `postcode_to` int(11) DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `courier_id` (`courier_id`),
	  KEY `country_id` (`country_id`),
	  CONSTRAINT `courier_remote_area_ibfk_1` FOREIGN KEY (`courier_id`) REFERENCES `courier` (`id`),
	  CONSTRAINT `courier_remote_area_ibfk_2` FOREIGN KEY (`country_id`) REFERENCES `country` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=1385670 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### courier\_to\_country

```

	CREATE TABLE `courier_to_country` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `country_id` int(11) NOT NULL,
	  `courier_id` int(11) NOT NULL,
	  `active_id` int(11) NOT NULL DEFAULT '1',
	  `courier_zone_id` int(11) DEFAULT NULL,
	  `preferred` int(11) NOT NULL DEFAULT '1',
	  `declare_multiplier` decimal(4,2) NOT NULL DEFAULT '1.00',
	  `shipping_multiplier` decimal(4,2) NOT NULL DEFAULT '1.00',
	  `delivery_signature` int(11) DEFAULT NULL,
	  `delivery_description` varchar(200) DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `country_id` (`country_id`),
	  KEY `courier_id` (`courier_id`),
	  KEY `active_id` (`active_id`),
	  KEY `courier_zone_id` (`courier_zone_id`),
	  CONSTRAINT `courier_to_country_ibfk_1` FOREIGN KEY (`country_id`) REFERENCES `country` (`id`),
	  CONSTRAINT `courier_to_country_ibfk_2` FOREIGN KEY (`courier_id`) REFERENCES `courier` (`id`),
	  CONSTRAINT `courier_to_country_ibfk_4` FOREIGN KEY (`active_id`) REFERENCES `active` (`id`),
	  CONSTRAINT `courier_to_country_ibfk_5` FOREIGN KEY (`courier_zone_id`) REFERENCES `courier_zone` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=6255 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### courier\_type

> 物流方式 (直发， 货代， Advanced）

```

	CREATE TABLE `courier_type` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(32) NOT NULL,
	  `value` tinyint(4) NOT NULL,
	  PRIMARY KEY (`id`),
	  UNIQUE KEY `name` (`name`)
	) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### courier\_zone

> 物流渠道???

```

	CREATE TABLE `courier_zone` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `courier_id` int(11) NOT NULL,
	  `zone_name` varchar(255) NOT NULL,
	  `description` varchar(255) DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `courier_id` (`courier_id`),
	  CONSTRAINT `courier_zone_ibfk_1` FOREIGN KEY (`courier_id`) REFERENCES `courier` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=547 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### cron\_job

> 定时任务

```

	CREATE TABLE `cron_job` (
	  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
	  `name` varchar(50) DEFAULT NULL,
	  `schedule` varchar(11) DEFAULT NULL,				// 频率 （现有tenmin， hourly, daily, weekly
	  `desc` varchar(200) DEFAULT NULL,
	  `data` varchar(2000) DEFAULT NULL COMMENT 'configuration data',
	  `status` int(11) DEFAULT '1' COMMENT 'Job status: 0-disabled, 1-waiting, 2-running, 3-finished, 9-error',
	  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
	  `date_create` datetime NOT NULL,
	  PRIMARY KEY (`id`),
	  UNIQUE KEY `name` (`name`)
	) ENGINE=InnoDB AUTO_INCREMENT=39 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### cron\_job\_history

> 定时任务历史记录

```

	CREATE TABLE `cron_job_history` (
	  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
	  `job_id` int(11) unsigned NOT NULL,				// =>cron_job.id
	  `result` varchar(2000) DEFAULT NULL,
	  `output` text,
	  `error` varchar(2000) DEFAULT NULL,
	  `start_time` datetime NOT NULL,
	  `end_time` datetime DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `job_id` (`job_id`),
	  CONSTRAINT `fk_cron_job_id` FOREIGN KEY (`job_id`) REFERENCES `cron_job` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=427177 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### currency

> 币种

```

	CREATE TABLE `currency` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `title` varchar(32) DEFAULT NULL,
	  `code` char(3) DEFAULT NULL,
	  `symbol_left` varchar(12) DEFAULT NULL,
	  `symbol_right` varchar(12) DEFAULT NULL,
	  `decimal_point` char(1) DEFAULT NULL,
	  `thousands_point` char(1) DEFAULT NULL,
	  `decimal_places` char(1) DEFAULT NULL,
	  `value` decimal(19,8) DEFAULT NULL,
	  `last_updated` datetime DEFAULT NULL,
	  `source` int(11) DEFAULT NULL,
	  `value_rmb` decimal(19,8) DEFAULT NULL,
	  `markup` decimal(6,3) NOT NULL DEFAULT '1.000',
	  PRIMARY KEY (`id`),
	  KEY `code` (`code`)
	) ENGINE=InnoDB AUTO_INCREMENT=15 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### currency\_history

> 各币种的汇率修改历史记录

```

	CREATE TABLE `currency_history` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `currencies_id` int(11) DEFAULT NULL,		// => currency.id
	  `value` decimal(19,8) DEFAULT NULL,
	  `last_updated` datetime DEFAULT NULL,
	  `source` int(11) DEFAULT NULL,			// ???
	  `value_rmb` decimal(19,8) DEFAULT NULL,	// 汇率
	  `markup` decimal(6,3) DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `currencies_id` (`currencies_id`),
	  CONSTRAINT `currency_history_ibfk_1` FOREIGN KEY (`currencies_id`) REFERENCES `currency` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=422371 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### customer

> 用户

```

	CREATE TABLE `customer` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `cluster_id` int(11) NOT NULL DEFAULT '1',		// ??? 没有找到对应的表
	  `firstname` varchar(32) DEFAULT NULL,
	  `lastname` varchar(32) DEFAULT NULL,
	  `email_address` varchar(96) DEFAULT NULL,
	  `default_address_id` int(11) DEFAULT NULL,
	  `password` varchar(40) DEFAULT NULL,
	  `newsletter` char(1) DEFAULT NULL,
	  `status_id` int(11) NOT NULL DEFAULT '1',
	  `price_level` int(11) NOT NULL DEFAULT '0',
	  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
	  PRIMARY KEY (`id`),
	  UNIQUE KEY `customers_email_address` (`email_address`),
	  KEY `customers_default_address_id` (`default_address_id`),
	  KEY `customers_status` (`status_id`),
	  KEY `cluster_id` (`cluster_id`),
	  CONSTRAINT `customer_ibfk_1` FOREIGN KEY (`status_id`) REFERENCES `customer_status` (`id`),
	  CONSTRAINT `customer_ibfk_2` FOREIGN KEY (`default_address_id`) REFERENCES `address_book` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=1937978 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### customer\_api\_access [已废弃]

> 表中一条数据

```

	CREATE TABLE `customer_api_access` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `customer_id` int(11) NOT NULL,
	  `api_id` int(11) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `api_id` (`api_id`),
	  KEY `customer_api_id` (`customer_id`) USING BTREE,
	  CONSTRAINT `customer_api_access_ibfk_1` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
	  CONSTRAINT `customer_api_access_ibfk_2` FOREIGN KEY (`api_id`) REFERENCES `api` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### customer\_api\_log [已废弃]

> 表中无数据

```

	CREATE TABLE `customer_api_log` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `customer_id` int(11) DEFAULT NULL,
	  `customer_key_id` int(11) DEFAULT NULL,
	  `remote_ip` int(11) DEFAULT NULL,
	  `status` int(11) NOT NULL,
	  `input` text NOT NULL,
	  `output` text NOT NULL,
	  `date_create` datetime NOT NULL,
	  `execution_time` int(11) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `customer_id` (`customer_id`),
	  CONSTRAINT `customer_api_log_ibfk_1` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

-- ----------------------------
### customer\_avatar

> 用户头像  // id与customer_id唯一

```

	CREATE TABLE `customer_avatar` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `customer_id` int(11) NOT NULL,
	  `force_mistery_man` int(11) NOT NULL,			// ???
	  `data` blob,
	  PRIMARY KEY (`id`),
	  UNIQUE KEY `customer_id` (`customer_id`),
	  KEY `force_mistery_man` (`force_mistery_man`),
	  CONSTRAINT `customer_avatar_ibfk_1` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
	  CONSTRAINT `customer_avatar_ibfk_2` FOREIGN KEY (`force_mistery_man`) REFERENCES `active` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=412 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### customer_index [已废弃]

>

```

	CREATE TABLE `customer_index` (
	  `id` bigint(10) unsigned NOT NULL COMMENT 'customer.id',
	  `weight` int(10) unsigned NOT NULL,
	  `query` varchar(3072) NOT NULL,
	  KEY `query` (`query`(255))
	) ENGINE=InnoDB DEFAULT CHARSET=utf8 CONNECTION='sphinx://127.0.0.1:9312/c_delta:c_index';

```

-- ----------------------------
### customer_info

>

```

	CREATE TABLE `customer_info` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `customer_id` int(11) NOT NULL DEFAULT '0',
	  `date_of_last_logon` datetime DEFAULT NULL,
	  `number_of_logons` int(5) DEFAULT NULL,						// 登录次数
	  `date_account_created` datetime DEFAULT NULL,					// 创建时间
	  `date_account_last_modified` datetime DEFAULT NULL,			// 最后修改时间
	  `global_product_notifications` int(1) DEFAULT '0',			// 大部分为null，一小部分为1
	  `recive_promotional_email` tinyint(4) NOT NULL DEFAULT '1',	// 是否接收促销邮件
	  `email_delivery_problem` int(11) NOT NULL DEFAULT '0',		// 邮件投递问题
	  `email_problem_date` datetime DEFAULT NULL,
	  `email_problem_expire` datetime DEFAULT NULL,
	  `player` varchar(40) DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `customer_id` (`customer_id`),
	  KEY `recive_promotional_email` (`recive_promotional_email`),
	  CONSTRAINT `customer_info_ibfk_1` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=1885934 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### customer_key

```

	CREATE TABLE `customer_key` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `customer_id` int(11) NOT NULL,
	  `api_key` varchar(45) NOT NULL,
	  `active_id` int(11) NOT NULL,
	  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
	  `date_create` datetime NOT NULL,
	  PRIMARY KEY (`id`),
	  UNIQUE KEY `key` (`api_key`),
	  KEY `customer_id` (`customer_id`),
	  KEY `active_id` (`active_id`),
	  CONSTRAINT `customer_key_ibfk_1` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
	  CONSTRAINT `customer_key_ibfk_2` FOREIGN KEY (`active_id`) REFERENCES `active` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=9269 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### customer_log

> 

```

	CREATE TABLE `customer_log` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `customer_id` int(11) NOT NULL,
	  `admin_user_id` int(11) DEFAULT NULL,
	  `field` varchar(60) NOT NULL,
	  `old_value` varchar(60) NOT NULL,
	  `new_value` varchar(60) NOT NULL,
	  `create_date` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
	  PRIMARY KEY (`id`),
	  KEY `customer_log_ibfk_1` (`customer_id`),
	  KEY `customer_log_ibfk_2` (`admin_user_id`),
	  CONSTRAINT `customer_log_ibfk_1` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
	  CONSTRAINT `customer_log_ibfk_2` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=2274644 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### customer_note

>

```

	CREATE TABLE `customer_note` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `customer_id` int(11) NOT NULL,
	  `created_date` datetime NOT NULL,
	  `admin_user_id` int(11) NOT NULL,
	  `active_id` int(11) NOT NULL,
	  `note` mediumtext NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `customer_note_admin_user_FK` (`admin_user_id`),
	  KEY `customer_note_active_FK` (`active_id`),
	  KEY `customer_note_customer_FK` (`customer_id`),
	  CONSTRAINT `customer_note_active_FK` FOREIGN KEY (`active_id`) REFERENCES `active` (`id`),
	  CONSTRAINT `customer_note_admin_user_FK` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`),
	  CONSTRAINT `customer_note_customer_FK` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=289 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### customer_status

> 用户状态

```

	CREATE TABLE `customer_status` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(30) NOT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=15 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### <font color="gray">customer\_to\_curier [已废弃]</font>

> 空表

```

	CREATE TABLE `customer_to_curier` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `customer_id` int(11) DEFAULT NULL,
	  `curier_id` int(11) NOT NULL,
	  `site_id` int(11) NOT NULL,
	  `order_id` int(11) DEFAULT NULL,
	  `address_book_id` int(11) DEFAULT NULL,
	  `additional_data` varchar(255) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `customer_id` (`customer_id`),
	  KEY `curier_id` (`curier_id`),
	  KEY `site_id` (`site_id`),
	  KEY `order_id` (`order_id`),
	  KEY `address_book_id` (`address_book_id`),
	  CONSTRAINT `customer_to_curier_ibfk_1` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
	  CONSTRAINT `customer_to_curier_ibfk_2` FOREIGN KEY (`curier_id`) REFERENCES `courier` (`id`),
	  CONSTRAINT `customer_to_curier_ibfk_3` FOREIGN KEY (`site_id`) REFERENCES `site` (`id`),
	  CONSTRAINT `customer_to_curier_ibfk_4` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`),
	  CONSTRAINT `customer_to_curier_ibfk_5` FOREIGN KEY (`address_book_id`) REFERENCES `address_book` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

-- ----------------------------
### customer\_to\_flag

>

```

	CREATE TABLE `customer_to_flag` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `customer_id` int(11) NOT NULL,
	  `flag_id` int(11) NOT NULL,
	  `admin_user_id` int(11) NOT NULL,
	  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
	  PRIMARY KEY (`id`),
	  KEY `customer_to_flag_ibfk_1` (`customer_id`),
	  KEY `customer_to_flag_ibfk_2` (`flag_id`),
	  KEY `customer_to_flag_ibfk_3` (`admin_user_id`),
	  CONSTRAINT `customer_to_flag_ibfk_1` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
	  CONSTRAINT `customer_to_flag_ibfk_2` FOREIGN KEY (`flag_id`) REFERENCES `flag` (`id`),
	  CONSTRAINT `customer_to_flag_ibfk_3` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=1236516 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### <font color="gray">_custom\_chart [已废弃]_</font>

>

```

	CREATE TABLE `custom_chart` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `admin_saved_query_id` int(11) DEFAULT NULL,
	  `type` varchar(10) NOT NULL DEFAULT 'pie',
	  `title` varchar(64) DEFAULT NULL,
	  `col1` varchar(16) NOT NULL DEFAULT 'counter',
	  `col2` varchar(128) NOT NULL DEFAULT 'counter',
	  `visible` tinyint(4) NOT NULL DEFAULT '1',
	  `width` int(11) NOT NULL DEFAULT '400',
	  `height` int(11) NOT NULL DEFAULT '300',
	  `col_offset` smallint(6) NOT NULL DEFAULT '0',
	  `scope` smallint(6) NOT NULL DEFAULT '0',
	  PRIMARY KEY (`id`),
	  KEY `saved_query_id` (`admin_saved_query_id`),
	  CONSTRAINT `custom_chart_ibfk_1` FOREIGN KEY (`admin_saved_query_id`) REFERENCES `admin_saved_query` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

-- ----------------------------
### cybs

> ???

```

	CREATE TABLE `cybs` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `status_id` int(11) NOT NULL,
	  `order_id` int(11) NOT NULL,
	  `request_id` varchar(22) NOT NULL,
	  `reconciliation_id` varchar(22) DEFAULT NULL,
	  `transaction_type` varchar(14) NOT NULL,
	  `decision` varchar(15) NOT NULL,
	  `reason_code` int(11) NOT NULL,
	  `currency_id` int(11) NOT NULL,
	  `amount` decimal(14,2) NOT NULL,
	  `cc_amount` decimal(14,2) NOT NULL,
	  `cc_reason_code` int(11) NOT NULL,
	  `cc_authorization_code` varchar(22) NOT NULL,
	  `cc_authorized_date_time` datetime NOT NULL,
	  `payment_option` varchar(5) NOT NULL,
	  `card_card_type` int(11) DEFAULT NULL,
	  `card_expiration_month` int(11) DEFAULT NULL,
	  `card_expiration_year` int(11) DEFAULT NULL,
	  `card_account_number` varchar(16) DEFAULT NULL,
	  `bill_address_id` int(11) DEFAULT NULL,
	  `ship_address_id` int(11) DEFAULT NULL,
	  `ip_address` int(10) unsigned DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `reconciliation_id` (`reconciliation_id`),
	  KEY `order_id` (`order_id`),
	  KEY `request_id` (`request_id`),
	  KEY `bill_address_id` (`bill_address_id`),
	  KEY `ship_address_id` (`ship_address_id`),
	  KEY `currency_id` (`currency_id`),
	  KEY `status_id` (`status_id`),
	  CONSTRAINT `cybs_ibfk_1` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`),
	  CONSTRAINT `cybs_ibfk_2` FOREIGN KEY (`currency_id`) REFERENCES `currency` (`id`),
	  CONSTRAINT `cybs_ibfk_3` FOREIGN KEY (`bill_address_id`) REFERENCES `address_book` (`id`),
	  CONSTRAINT `cybs_ibfk_4` FOREIGN KEY (`ship_address_id`) REFERENCES `address_book` (`id`),
	  CONSTRAINT `cybs_ibfk_5` FOREIGN KEY (`status_id`) REFERENCES `cybs_status` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=138501 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### cybs\_collect

> ???

```
	CREATE TABLE `cybs_collect` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `cybs_id` int(11) NOT NULL,
	  `order_id` int(11) NOT NULL,
	  `status_id` int(11) NOT NULL,
	  `date` datetime NOT NULL,
	  `amount` decimal(7,2) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `order_id` (`order_id`),
	  KEY `cybs_id` (`cybs_id`),
	  KEY `status_id` (`status_id`),
	  CONSTRAINT `cybs_collect_ibfk_1` FOREIGN KEY (`cybs_id`) REFERENCES `cybs` (`id`),
	  CONSTRAINT `cybs_collect_ibfk_2` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`),
	  CONSTRAINT `cybs_collect_ibfk_3` FOREIGN KEY (`status_id`) REFERENCES `cybs_status` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=47839 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### cybs\_status

> ???

```

	CREATE TABLE `cybs_status` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(32) NOT NULL,
	  `customer_name` varchar(32) NOT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### event\_group

>

```

	CREATE TABLE `event_group` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `date_create` datetime NOT NULL,
	  `admin_user_id` int(11) NOT NULL,
	  `interface` varchar(255) DEFAULT NULL,	// ???
	  PRIMARY KEY (`id`),
	  KEY `admin_user_id` (`admin_user_id`),
	  KEY `date_create` (`date_create`),
	  CONSTRAINT `event_group_ibfk_1` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=840989 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### event\_log

```

	CREATE TABLE `event_log` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `customer_id` int(11) DEFAULT NULL,
	  `admin_user_id` int(11) DEFAULT NULL,
	  `order_id` int(11) DEFAULT NULL,
	  `site_id` int(11) NOT NULL DEFAULT '1',
	  `template_page_id` int(11) DEFAULT NULL,
	  `custom_template` text,
	  `date_create` datetime NOT NULL,
	  `date_received` datetime DEFAULT NULL,
	  `date_sent` datetime DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `customer_id` (`customer_id`),
	  KEY `admin_user_id` (`admin_user_id`),
	  KEY `order_id` (`order_id`),
	  KEY `template_page_id` (`template_page_id`),
	  KEY `date_create` (`date_create`),
	  KEY `date_received` (`date_received`),
	  KEY `date_sent` (`date_sent`),
	  KEY `site_id` (`site_id`),
	  CONSTRAINT `event_log_ibfk_1` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
	  CONSTRAINT `event_log_ibfk_2` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`),
	  CONSTRAINT `event_log_ibfk_3` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`),
	  CONSTRAINT `event_log_ibfk_4` FOREIGN KEY (`template_page_id`) REFERENCES `page` (`id`),
	  CONSTRAINT `event_log_ibfk_5` FOREIGN KEY (`site_id`) REFERENCES `site` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=22488402 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### <font color="gray">_event\_log\_to\_views [已作废]_</font>

> 空表

```

	CREATE TABLE `event_log_to_views` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `event_log_id` int(11) NOT NULL,
	  `admin_user_id` int(11) DEFAULT NULL,
	  `customer_id` int(11) DEFAULT NULL,
	  `date_viewed` datetime DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `event_log_id` (`event_log_id`),
	  KEY `admin_user_id` (`admin_user_id`),
	  KEY `customer_id` (`customer_id`),
	  KEY `date_viewed` (`date_viewed`),
	  CONSTRAINT `event_log_to_views_ibfk_1` FOREIGN KEY (`event_log_id`) REFERENCES `event_log` (`id`),
	  CONSTRAINT `event_log_to_views_ibfk_2` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`),
	  CONSTRAINT `event_log_to_views_ibfk_3` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

-- ----------------------------
### event\_to\_group

>

```

	CREATE TABLE `event_to_group` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `event_group_id` int(11) NOT NULL,
	  `event_log_id` int(11) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `event_group_id` (`event_group_id`),
	  KEY `event_id` (`event_log_id`),
	  CONSTRAINT `event_to_group_ibfk_1` FOREIGN KEY (`event_log_id`) REFERENCES `event_log` (`id`),
	  CONSTRAINT `event_to_group_ibfk_2` FOREIGN KEY (`event_group_id`) REFERENCES `event_group` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=3693838 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### export\_history

>

```

	CREATE TABLE `export_history` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `date` datetime NOT NULL,
	  `contents` longblob NOT NULL,
	  `comment` varchar(50) DEFAULT NULL,		// 现存数据中 comment都为null
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=98442 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### external\_keyword

>

```

	CREATE TABLE `external_keyword` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `affiliate_keyword` varchar(32) DEFAULT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### flag

>

```

	CREATE TABLE `flag` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `type` int(11) NOT NULL,
	  `name` varchar(60) NOT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=51 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### historical_stock

> ???

```

	CREATE TABLE `historical_stock` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `stock` decimal(15,2) NOT NULL,
	  `po` decimal(15,2) NOT NULL,
	  `when` datetime NOT NULL,
	  `instock_products` int(11) NOT NULL,
	  `total_quantity` int(11) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `when` (`when`)
	) ENGINE=InnoDB AUTO_INCREMENT=4146 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### image

> ???

```

	CREATE TABLE `image` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `filename` varchar(40) NOT NULL,
	  `description` varchar(40) NOT NULL,
	  `height` int(11) NOT NULL,
	  `width` int(11) NOT NULL,
	  `filesize` int(11) NOT NULL,
	  `quality` int(11) NOT NULL,
	  `date_uploaded` datetime NOT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=2648 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### interface\_object\_list

>

```

	CREATE TABLE `interface_object_list` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(50) NOT NULL,
	  PRIMARY KEY (`id`),
	  UNIQUE KEY `name` (`name`)
	) ENGINE=InnoDB AUTO_INCREMENT=16 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### interface\_permission\_list

> 

```

	CREATE TABLE `interface_permission_list` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(50) NOT NULL,
	  PRIMARY KEY (`id`),
	  UNIQUE KEY `name` (`name`)
	) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### interface\_to\_menu

>

```

	CREATE TABLE `interface_to_menu` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(60) NOT NULL,
	  `file_name` varchar(64) NOT NULL,
	  `menu_id` int(11) NOT NULL,
	  `active_id` int(11) NOT NULL,
	  `sort_id` int(11) NOT NULL,
	  `get_atribute` varchar(255) DEFAULT NULL,
	  `default_permission_id` int(11) NOT NULL DEFAULT '2',
	  `site_id` int(11) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `category_id` (`menu_id`),
	  KEY `active_id` (`active_id`),
	  KEY `default_permission_id` (`default_permission_id`),
	  KEY `site_id` (`site_id`),
	  CONSTRAINT `interface_to_menu_ibfk_2` FOREIGN KEY (`active_id`) REFERENCES `active` (`id`),
	  CONSTRAINT `interface_to_menu_ibfk_3` FOREIGN KEY (`default_permission_id`) REFERENCES `interface_permission_list` (`id`),
	  CONSTRAINT `interface_to_menu_ibfk_4` FOREIGN KEY (`menu_id`) REFERENCES `menu` (`id`),
	  CONSTRAINT `interface_to_menu_ibfk_5` FOREIGN KEY (`site_id`) REFERENCES `site` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=1582 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### iptocountry

>

```

	CREATE TABLE `iptocountry` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `ipfrom` bigint(20) NOT NULL,
	  `ipto` bigint(20) NOT NULL,
	  `iso_country` varchar(2) DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `ipfrom` (`ipfrom`),
	  KEY `ipto` (`ipto`)
	) ENGINE=InnoDB AUTO_INCREMENT=47406829 DEFAULT CHARSET=utf8;

```

-- ----------------------------
### keyword

> ???

```
	CREATE TABLE `keyword` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `keyword` varchar(100) DEFAULT NULL,
	  `url` varchar(150) DEFAULT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=427 DEFAULT CHARSET=utf8;
```

-- ----------------------------
### menu

>
```
	CREATE TABLE `menu` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `file_name` varchar(50) DEFAULT NULL,
	  `name` varchar(64) NOT NULL,
	  `get_atribute` varchar(255) DEFAULT NULL,
	  `active` int(11) NOT NULL DEFAULT '0',
	  `parent_id` int(11) DEFAULT '0',
	  `level` int(11) NOT NULL DEFAULT '0',
	  `sort_id` int(11) NOT NULL,
	  `site_id` int(11) NOT NULL,
	  PRIMARY KEY (`id`),
	  KEY `active` (`active`),
	  KEY `parent_id` (`parent_id`),
	  KEY `sort_id` (`sort_id`),
	  KEY `site_id` (`site_id`),
	  CONSTRAINT `menu_ibfk_1` FOREIGN KEY (`parent_id`) REFERENCES `menu` (`id`),
	  CONSTRAINT `menu_ibfk_2` FOREIGN KEY (`site_id`) REFERENCES `site` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=88 DEFAULT CHARSET=utf8;
```

-- ----------------------------
### meta\_tag

>

```
	DROP TABLE IF EXISTS `meta_tag`;
	CREATE TABLE `meta_tag` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `category_id` int(11) NOT NULL DEFAULT '0',
	  `keywords` varchar(255) DEFAULT NULL,
	  `description` varchar(255) DEFAULT NULL,
	  `title` varchar(255) DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `categories_id` (`category_id`),
	  KEY `title` (`title`),
	  KEY `keywords` (`keywords`),
	  KEY `description` (`description`),
	  CONSTRAINT `meta_tag_ibfk_1` FOREIGN KEY (`category_id`) REFERENCES `category` (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=364 DEFAULT CHARSET=utf8;
```

-- ----------------------------
### most_search

>

```
	CREATE TABLE `most_search` (
	  `id` int(32) NOT NULL AUTO_INCREMENT,
	  `name` varchar(255) DEFAULT NULL,
	  `count` int(11) NOT NULL DEFAULT '0',
	  `hits` int(11) NOT NULL DEFAULT '0',
	  `last_searched` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00' ON UPDATE CURRENT_TIMESTAMP,
	  PRIMARY KEY (`id`),
	  KEY `most_search_count` (`count`,`name`)
	) ENGINE=InnoDB AUTO_INCREMENT=762573 DEFAULT CHARSET=utf8;
```

-- ----------------------------
### move_status

> 

```
	CREATE TABLE `move_status` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `label` varchar(50) NOT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;
```

-- ----------------------------
### newsletter_subscriber

```
	CREATE TABLE `newsletter_subscriber` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `email_address` varchar(96) NOT NULL,
	  `firstname` varchar(32) NOT NULL,
	  `lastname` varchar(32) NOT NULL,
	  `recive_promotional_email` tinyint(4) NOT NULL DEFAULT '1',
	  `email_delivery_problem` tinyint(4) NOT NULL DEFAULT '1',
	  PRIMARY KEY (`id`),
	  UNIQUE KEY `email_address` (`email_address`)
	) ENGINE=InnoDB AUTO_INCREMENT=25506 DEFAULT CHARSET=utf8;
```

-- ----------------------------
### occ_log

> 

```
CREATE TABLE `occ_log` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `old_courier_id` int(11) NOT NULL,
  `new_courier_id` int(11) NOT NULL,
  `order_id` int(11) NOT NULL,
  `date_created` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00' ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `old_courier_id` (`old_courier_id`),
  KEY `new_courier_id` (`new_courier_id`),
  KEY `order_id` (`order_id`),
  CONSTRAINT `occ_log_ibfk_1` FOREIGN KEY (`old_courier_id`) REFERENCES `courier` (`id`),
  CONSTRAINT `occ_log_ibfk_2` FOREIGN KEY (`new_courier_id`) REFERENCES `courier` (`id`),
  CONSTRAINT `occ_log_ibfk_3` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=21445 DEFAULT CHARSET=utf8;
```

-- ----------------------------
### openid_to_customer
-- ----------------------------
DROP TABLE IF EXISTS `openid_to_customer`;
CREATE TABLE `openid_to_customer` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `open_id` varchar(100) NOT NULL,
  `customer_id` int(11) NOT NULL,
  `hash` bigint(20) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `open_id` (`open_id`),
  KEY `customer_id` (`customer_id`),
  KEY `hash` (`hash`),
  CONSTRAINT `openid_to_customer_ibfk_1` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=333 DEFAULT CHARSET=utf8;

-- ----------------------------
### openid_to_user
-- ----------------------------
DROP TABLE IF EXISTS `openid_to_user`;
CREATE TABLE `openid_to_user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `open_id` varchar(100) NOT NULL,
  `users_id` int(11) NOT NULL,
  `hash` bigint(20) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `users_id` (`users_id`),
  CONSTRAINT `openid_to_user_ibfk_1` FOREIGN KEY (`users_id`) REFERENCES `admin_user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=747 DEFAULT CHARSET=utf8;

-- ----------------------------
### order
-- ----------------------------
DROP TABLE IF EXISTS `order`;
CREATE TABLE `order` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `customer_id` int(11) NOT NULL,
  `payment_method` varchar(32) DEFAULT NULL,
  `order_status_id` int(11) NOT NULL DEFAULT '0',
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  `date_purchased` datetime DEFAULT NULL,
  `date_payment` datetime DEFAULT NULL,
  `date_finished` datetime DEFAULT NULL,
  `date_warehouse` datetime DEFAULT NULL,
  `currency` char(3) DEFAULT NULL,
  `currency_value` decimal(14,6) DEFAULT NULL,
  `delivery_tracking_nr` varchar(50) DEFAULT NULL,
  `shipping_value` decimal(15,4) NOT NULL,
  `shipping_value_declared` decimal(15,4) DEFAULT NULL,
  `courier_id` int(11) NOT NULL,
  `subtotal_value` decimal(14,6) NOT NULL,
  `customers_address_id` int(11) NOT NULL,
  `delivery_address_id` int(11) NOT NULL,
  `billing_address_id` int(11) NOT NULL,
  `ip` int(10) unsigned DEFAULT NULL,
  `coupon_id` int(11) DEFAULT NULL,
  `insurance` decimal(14,6) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `customers_id` (`customer_id`),
  KEY `orders_status` (`order_status_id`),
  KEY `currency` (`currency`),
  KEY `date_purchased` (`date_purchased`),
  KEY `last_modified` (`last_modified`),
  KEY `sync_idx` (`id`,`last_modified`),
  KEY `delivery_address_id` (`delivery_address_id`),
  KEY `shipping_agent_id` (`courier_id`),
  KEY `order_ibfk_5` (`customers_address_id`),
  KEY `order_ibfk_6` (`billing_address_id`),
  KEY `coupon_id` (`coupon_id`),
  CONSTRAINT `order_ibfk_1` FOREIGN KEY (`order_status_id`) REFERENCES `order_status` (`id`),
  CONSTRAINT `order_ibfk_2` FOREIGN KEY (`delivery_address_id`) REFERENCES `address_book` (`id`),
  CONSTRAINT `order_ibfk_3` FOREIGN KEY (`courier_id`) REFERENCES `courier` (`id`),
  CONSTRAINT `order_ibfk_4` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
  CONSTRAINT `order_ibfk_5` FOREIGN KEY (`customers_address_id`) REFERENCES `address_book` (`id`),
  CONSTRAINT `order_ibfk_6` FOREIGN KEY (`billing_address_id`) REFERENCES `address_book` (`id`),
  CONSTRAINT `order_ibfk_7` FOREIGN KEY (`coupon_id`) REFERENCES `coupon` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2907807 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_affiliate_pixel
-- ----------------------------
DROP TABLE IF EXISTS `order_affiliate_pixel`;
CREATE TABLE `order_affiliate_pixel` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_id` int(11) NOT NULL,
  `type` tinyint(4) NOT NULL,
  `date` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `ip` int(11) NOT NULL,
  `line` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`),
  KEY `order_id` (`order_id`),
  CONSTRAINT `order_affiliate_pixel_ibfk_1` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=462872 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_comment
-- ----------------------------
DROP TABLE IF EXISTS `order_comment`;
CREATE TABLE `order_comment` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_id` int(11) NOT NULL,
  `comment` text NOT NULL,
  `last_update` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `order_id` (`order_id`),
  CONSTRAINT `order_comment_ibfk_1` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=394773 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_courier_change
-- ----------------------------
DROP TABLE IF EXISTS `order_courier_change`;
CREATE TABLE `order_courier_change` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `event_log_id` int(11) NOT NULL,
  `price_before` decimal(15,4) NOT NULL,
  `price_after` decimal(15,4) NOT NULL,
  `before_courier_id` int(11) NOT NULL,
  `after_courier_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `order_courier_change_ibfk_1` (`event_log_id`),
  KEY `order_courier_change_ibfk_2` (`before_courier_id`),
  KEY `order_courier_change_ibfk_3` (`after_courier_id`),
  CONSTRAINT `order_courier_change_ibfk_1` FOREIGN KEY (`event_log_id`) REFERENCES `event_log` (`id`),
  CONSTRAINT `order_courier_change_ibfk_2` FOREIGN KEY (`before_courier_id`) REFERENCES `courier` (`id`),
  CONSTRAINT `order_courier_change_ibfk_3` FOREIGN KEY (`after_courier_id`) REFERENCES `courier` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=95261 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_group
-- ----------------------------
DROP TABLE IF EXISTS `order_group`;
CREATE TABLE `order_group` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(60) NOT NULL,
  `level` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=202 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_group_description
-- ----------------------------
DROP TABLE IF EXISTS `order_group_description`;
CREATE TABLE `order_group_description` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_group_id` int(11) NOT NULL,
  `long_description` text NOT NULL,
  `short_description` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `order_group_description_ibfk_1` (`order_group_id`),
  CONSTRAINT `order_group_description_ibfk_1` FOREIGN KEY (`order_group_id`) REFERENCES `order_group` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=210 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_index
-- ----------------------------
DROP TABLE IF EXISTS `order_index`;
CREATE TABLE `order_index` (
  `id` int(10) unsigned NOT NULL COMMENT 'order.id',
  `weight` int(10) unsigned NOT NULL,
  `query` varchar(3072) NOT NULL,
  KEY `query` (`query`(255))
) ENGINE=InnoDB DEFAULT CHARSET=utf8 CONNECTION='sphinx://127.0.0.1:9312/o_delta:o_index';

-- ----------------------------
### order_product
-- ----------------------------
DROP TABLE IF EXISTS `order_product`;
CREATE TABLE `order_product` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_id` int(11) NOT NULL DEFAULT '0',
  `product_id` int(11) DEFAULT NULL,
  `product_alternative` varchar(128) DEFAULT NULL,
  `product_price` decimal(15,4) NOT NULL DEFAULT '0.0000',
  `declared_price` decimal(15,4) DEFAULT '0.0000',
  `avg_buy_price` decimal(15,4) DEFAULT NULL,
  `product_quantity` int(11) NOT NULL DEFAULT '0',
  `product_original_quantity` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`),
  KEY `products_id` (`product_id`),
  KEY `order_id` (`order_id`),
  CONSTRAINT `order_product_ibfk_1` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`),
  CONSTRAINT `order_product_ibfk_2` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4768970 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_product_change
-- ----------------------------
DROP TABLE IF EXISTS `order_product_change`;
CREATE TABLE `order_product_change` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `event_log_id` int(11) NOT NULL,
  `product_id` int(11) NOT NULL,
  `price_before` decimal(15,4) NOT NULL,
  `price_after` decimal(15,4) NOT NULL,
  `q_before` int(11) NOT NULL,
  `q_after` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `order_product_change_ibfk_1` (`event_log_id`),
  KEY `order_product_change_ibfk_2` (`product_id`),
  CONSTRAINT `order_product_change_ibfk_1` FOREIGN KEY (`event_log_id`) REFERENCES `event_log` (`id`),
  CONSTRAINT `order_product_change_ibfk_2` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=92077 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_purchase
-- ----------------------------
DROP TABLE IF EXISTS `order_purchase`;
CREATE TABLE `order_purchase` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `vendor_id` int(11) NOT NULL,
  `order_purchase_status_id` int(11) NOT NULL,
  `export_history_id` int(11) DEFAULT NULL,
  `date_create` datetime NOT NULL,
  `date_arrive` datetime NOT NULL,
  `currency_id` int(11) NOT NULL,
  `currency_value` decimal(14,6) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `export_history_id` (`export_history_id`),
  KEY `vendor_id` (`vendor_id`),
  KEY `order_purchase_status_id` (`order_purchase_status_id`),
  KEY `order_purchase_ibfk_4` (`currency_id`),
  CONSTRAINT `order_purchase_ibfk_1` FOREIGN KEY (`vendor_id`) REFERENCES `vendor` (`id`),
  CONSTRAINT `order_purchase_ibfk_2` FOREIGN KEY (`order_purchase_status_id`) REFERENCES `order_purchase_status` (`id`),
  CONSTRAINT `order_purchase_ibfk_3` FOREIGN KEY (`export_history_id`) REFERENCES `export_history` (`id`),
  CONSTRAINT `order_purchase_ibfk_4` FOREIGN KEY (`currency_id`) REFERENCES `currency` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=129073 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_purchase_additional
-- ----------------------------
DROP TABLE IF EXISTS `order_purchase_additional`;
CREATE TABLE `order_purchase_additional` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_purchase_id` int(11) NOT NULL,
  `additional_description` text NOT NULL,
  `additional_amount` decimal(15,4) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `orders_purchase_id` (`order_purchase_id`),
  CONSTRAINT `order_purchase_additional_ibfk_1` FOREIGN KEY (`order_purchase_id`) REFERENCES `order_purchase` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=54747 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_purchase_delivery
-- ----------------------------
DROP TABLE IF EXISTS `order_purchase_delivery`;
CREATE TABLE `order_purchase_delivery` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_purchase_product_id` int(11) NOT NULL,
  `delivery_date` date NOT NULL,
  `statistical_delivery_date` date NOT NULL,
  `delivery_quantity` int(11) NOT NULL,
  `w2w_product_id` bigint(20) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `order_purchase_delivery_ibfk_2` (`w2w_product_id`),
  KEY `order_purchase_delivery_ibfk_1` (`order_purchase_product_id`),
  CONSTRAINT `order_purchase_delivery_ibfk_1` FOREIGN KEY (`order_purchase_product_id`) REFERENCES `order_purchase_product` (`id`),
  CONSTRAINT `order_purchase_delivery_ibfk_2` FOREIGN KEY (`w2w_product_id`) REFERENCES `warehouse_2_warehouse_product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=33144 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_purchase_product
-- ----------------------------
DROP TABLE IF EXISTS `order_purchase_product`;
CREATE TABLE `order_purchase_product` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_purchase_id` int(11) NOT NULL,
  `product_id` int(11) NOT NULL,
  `quantity` int(11) NOT NULL,
  `price` decimal(15,4) NOT NULL,
  `arrived_quantity` int(11) NOT NULL DEFAULT '0',
  `expected_delivery_date` date NOT NULL,
  PRIMARY KEY (`id`),
  KEY `orders_purchase_id` (`order_purchase_id`),
  KEY `products_id` (`product_id`),
  CONSTRAINT `order_purchase_product_ibfk_1` FOREIGN KEY (`order_purchase_id`) REFERENCES `order_purchase` (`id`),
  CONSTRAINT `order_purchase_product_ibfk_2` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=180732 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_purchase_status
-- ----------------------------
DROP TABLE IF EXISTS `order_purchase_status`;
CREATE TABLE `order_purchase_status` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `label` varchar(64) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_referrer
-- ----------------------------
DROP TABLE IF EXISTS `order_referrer`;
CREATE TABLE `order_referrer` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_id` int(11) NOT NULL,
  `referrer_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `order_id` (`order_id`),
  KEY `referrer_id` (`referrer_id`),
  CONSTRAINT `order_referrer_ibfk_1` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`),
  CONSTRAINT `order_referrer_ibfk_2` FOREIGN KEY (`referrer_id`) REFERENCES `referrer_site` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=236733 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_status
-- ----------------------------
DROP TABLE IF EXISTS `order_status`;
CREATE TABLE `order_status` (
  `id` int(11) NOT NULL,
  `name` varchar(60) DEFAULT NULL,
  `customer_status_name` varchar(60) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `idx_orders_status_name` (`name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- ----------------------------
### order_status_history
-- ----------------------------
DROP TABLE IF EXISTS `order_status_history`;
CREATE TABLE `order_status_history` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_id` int(11) NOT NULL DEFAULT '0',
  `order_status_id` int(5) NOT NULL DEFAULT '0',
  `date_added` datetime NOT NULL DEFAULT '0000-00-00 00:00:00',
  `customer_notified` int(1) DEFAULT '0',
  `comments` text,
  PRIMARY KEY (`id`),
  KEY `orders_id` (`order_id`),
  KEY `orders_status_id` (`order_status_id`),
  KEY `date_added` (`date_added`),
  CONSTRAINT `order_status_history_ibfk_1` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`),
  CONSTRAINT `order_status_history_ibfk_2` FOREIGN KEY (`order_status_id`) REFERENCES `order_status` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3662220 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_status_history_event
-- ----------------------------
DROP TABLE IF EXISTS `order_status_history_event`;
CREATE TABLE `order_status_history_event` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_id` int(11) NOT NULL,
  `event_log_id` int(11) NOT NULL,
  `old_orders_status_id` int(11) DEFAULT NULL,
  `new_orders_status_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `orders_id` (`order_id`),
  KEY `event_id` (`event_log_id`),
  KEY `old_orders_status_id` (`old_orders_status_id`),
  KEY `new_orders_status_id` (`new_orders_status_id`),
  CONSTRAINT `order_status_history_event_ibfk_1` FOREIGN KEY (`new_orders_status_id`) REFERENCES `order_status` (`id`),
  CONSTRAINT `order_status_history_event_ibfk_2` FOREIGN KEY (`old_orders_status_id`) REFERENCES `order_status` (`id`),
  CONSTRAINT `order_status_history_event_ibfk_3` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`),
  CONSTRAINT `order_status_history_event_ibfk_4` FOREIGN KEY (`event_log_id`) REFERENCES `event_log` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=16776449 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_status_to_group
-- ----------------------------
DROP TABLE IF EXISTS `order_status_to_group`;
CREATE TABLE `order_status_to_group` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_status_id` int(11) NOT NULL,
  `order_group_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `order_status_to_group_ibfk_1` (`order_status_id`),
  KEY `order_status_to_group_ibfk_2` (`order_group_id`),
  CONSTRAINT `order_status_to_group_ibfk_1` FOREIGN KEY (`order_status_id`) REFERENCES `order_status` (`id`),
  CONSTRAINT `order_status_to_group_ibfk_2` FOREIGN KEY (`order_group_id`) REFERENCES `order_group` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2494 DEFAULT CHARSET=utf8;

-- ----------------------------
### order_w2w
-- ----------------------------
DROP TABLE IF EXISTS `order_w2w`;
CREATE TABLE `order_w2w` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_id` int(11) NOT NULL,
  `w2w_id` int(11) NOT NULL,
  `event_log_id` int(11) NOT NULL,
  `direction_in` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `order_w2w_ibfk_1` (`order_id`),
  KEY `order_w2w_ibfk_2` (`w2w_id`),
  KEY `order_w2w_ibfk_4` (`direction_in`),
  CONSTRAINT `order_w2w_ibfk_1` FOREIGN KEY (`w2w_id`) REFERENCES `warehouse_2_warehouse` (`id`),
  CONSTRAINT `order_w2w_ibfk_2` FOREIGN KEY (`direction_in`) REFERENCES `active` (`id`),
  CONSTRAINT `order_w2w_ibfk_3` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1817872 DEFAULT CHARSET=utf8;

-- ----------------------------
### package
-- ----------------------------
DROP TABLE IF EXISTS `package`;
CREATE TABLE `package` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_id` int(11) NOT NULL,
  `weight_checkout` decimal(14,3) DEFAULT NULL,
  `weight` decimal(14,3) DEFAULT NULL,
  `weight_dimensional` decimal(14,3) DEFAULT NULL,
  `weight_courier_input` decimal(14,3) DEFAULT NULL,
  `weight_courier_charged` decimal(14,3) DEFAULT NULL,
  `length` int(11) DEFAULT NULL,
  `width` int(11) DEFAULT NULL,
  `height` int(11) DEFAULT NULL,
  `courier_billed_amount` decimal(15,4) DEFAULT NULL,
  `currency_id` int(11) DEFAULT NULL,
  `currency_value` decimal(14,6) DEFAULT NULL,
  `delivery_tracking_nr` varchar(50) DEFAULT NULL,
  `courier_billing_to` varchar(12) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `package_ibfk_1` (`order_id`),
  KEY `package_ibfk_2` (`currency_id`),
  KEY `courier_billing_to` (`courier_billing_to`),
  KEY `delivery_tracking_nr` (`delivery_tracking_nr`),
  CONSTRAINT `package_ibfk_1` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`),
  CONSTRAINT `package_ibfk_2` FOREIGN KEY (`currency_id`) REFERENCES `currency` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2389094 DEFAULT CHARSET=utf8;

-- ----------------------------
### package_tare
-- ----------------------------
DROP TABLE IF EXISTS `package_tare`;
CREATE TABLE `package_tare` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `weight` decimal(6,2) NOT NULL,
  `actual_fix` int(11) NOT NULL,
  `actual_percentage` int(11) NOT NULL,
  `dimensional_fix` int(11) NOT NULL,
  `dimensional_percentage` int(11) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=44 DEFAULT CHARSET=utf8;

-- ----------------------------
### page
-- ----------------------------
DROP TABLE IF EXISTS `page`;
CREATE TABLE `page` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `admin_user_id` int(11) NOT NULL DEFAULT '1',
  `date` datetime NOT NULL DEFAULT '0000-00-00 00:00:00',
  `content` longtext,
  `title` text,
  `url_path` varchar(100) DEFAULT NULL,
  `name` varchar(200) DEFAULT NULL,
  `modified` datetime DEFAULT NULL,
  `parent` int(11) DEFAULT NULL,
  `flag_auth_active_id` int(11) NOT NULL DEFAULT '1',
  `flag_sphinx_active_id` int(11) NOT NULL DEFAULT '0',
  `flag_noindex_active_id` int(11) NOT NULL DEFAULT '1',
  PRIMARY KEY (`id`),
  KEY `post_name` (`name`),
  KEY `type_status_date` (`date`,`id`),
  KEY `admin_user_id` (`admin_user_id`),
  KEY `flag_auth_active_id` (`flag_auth_active_id`),
  KEY `flag_sphinx_active_id` (`flag_sphinx_active_id`),
  KEY `flag_noindex_active_id` (`flag_noindex_active_id`),
  CONSTRAINT `page_ibfk_1` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`),
  CONSTRAINT `page_ibfk_2` FOREIGN KEY (`flag_auth_active_id`) REFERENCES `active` (`id`),
  CONSTRAINT `page_ibfk_3` FOREIGN KEY (`flag_sphinx_active_id`) REFERENCES `active` (`id`),
  CONSTRAINT `page_ibfk_4` FOREIGN KEY (`flag_noindex_active_id`) REFERENCES `active` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2025 DEFAULT CHARSET=utf8;

-- ----------------------------
### page_index
-- ----------------------------
DROP TABLE IF EXISTS `page_index`;
CREATE TABLE `page_index` (
  `id` int(10) unsigned NOT NULL COMMENT 'page.id',
  `weight` int(10) unsigned NOT NULL,
  `query` varchar(3072) NOT NULL,
  KEY `query` (`query`(255))
) ENGINE=InnoDB DEFAULT CHARSET=utf8 CONNECTION='sphinx://127.0.0.1:9312/p_delta:p_index';

-- ----------------------------
### page_to_page_category
-- ----------------------------
DROP TABLE IF EXISTS `page_to_page_category`;
CREATE TABLE `page_to_page_category` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `page_id` int(11) NOT NULL,
  `page_category_id` int(11) NOT NULL,
  `sort_value` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `page_id` (`page_id`),
  KEY `page_category_id` (`page_category_id`),
  KEY `sort_value` (`sort_value`),
  CONSTRAINT `page_to_page_category_ibfk_1` FOREIGN KEY (`page_id`) REFERENCES `page` (`id`),
  CONSTRAINT `page_to_page_category_ibfk_2` FOREIGN KEY (`page_category_id`) REFERENCES `page` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- ----------------------------
### payment
-- ----------------------------
DROP TABLE IF EXISTS `payment`;
CREATE TABLE `payment` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `date_create` datetime NOT NULL,
  `order_id` int(11) DEFAULT NULL,
  `customer_id` int(11) DEFAULT NULL,
  `admin_user_id` int(11) DEFAULT NULL,
  `description` text,
  `from_payment_list_id` int(11) NOT NULL,
  `to_payment_list_id` int(11) NOT NULL,
  `amount` decimal(15,4) NOT NULL,
  `exchange_rate` decimal(15,6) NOT NULL,
  `reference_id` varchar(100) DEFAULT NULL,
  `paypal_id` int(11) DEFAULT NULL,
  `currency_id` int(11) NOT NULL DEFAULT '1',
  `cybs_id` int(11) DEFAULT NULL,
  `skrill_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `date_create` (`date_create`),
  KEY `orders_id` (`order_id`),
  KEY `customer_id` (`customer_id`),
  KEY `admin_user_id` (`admin_user_id`),
  KEY `payment_ibfk_5` (`from_payment_list_id`),
  KEY `payment_ibfk_6` (`to_payment_list_id`),
  KEY `payment_ibfk_7` (`currency_id`),
  KEY `payment_ibfk_4` (`paypal_id`),
  KEY `cybs_id` (`cybs_id`),
  KEY `skrill_id` (`skrill_id`),
  CONSTRAINT `payment_ibfk_1` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`),
  CONSTRAINT `payment_ibfk_2` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
  CONSTRAINT `payment_ibfk_3` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`),
  CONSTRAINT `payment_ibfk_4` FOREIGN KEY (`paypal_id`) REFERENCES `paypal` (`id`),
  CONSTRAINT `payment_ibfk_5` FOREIGN KEY (`from_payment_list_id`) REFERENCES `payment_list` (`id`),
  CONSTRAINT `payment_ibfk_6` FOREIGN KEY (`to_payment_list_id`) REFERENCES `payment_list` (`id`),
  CONSTRAINT `payment_ibfk_7` FOREIGN KEY (`currency_id`) REFERENCES `currency` (`id`),
  CONSTRAINT `payment_ibfk_8` FOREIGN KEY (`cybs_id`) REFERENCES `cybs` (`id`),
  CONSTRAINT `payment_ibfk_9` FOREIGN KEY (`skrill_id`) REFERENCES `skrill` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1450440 DEFAULT CHARSET=utf8;

-- ----------------------------
### payment_balance
-- ----------------------------
DROP TABLE IF EXISTS `payment_balance`;
CREATE TABLE `payment_balance` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `customer_id` int(11) NOT NULL,
  `currency_id` int(11) NOT NULL,
  `balance` decimal(15,4) NOT NULL DEFAULT '0.0000',
  `ol_time_stamp` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  UNIQUE KEY `natural_key` (`customer_id`,`currency_id`),
  KEY `payment_balance_ibfk_1` (`customer_id`),
  KEY `payment_balance_ibfk_2` (`currency_id`),
  CONSTRAINT `payment_balance_ibfk_1` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
  CONSTRAINT `payment_balance_ibfk_2` FOREIGN KEY (`currency_id`) REFERENCES `currency` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=890258 DEFAULT CHARSET=utf8;

-- ----------------------------
### payment_ledger
-- ----------------------------
DROP TABLE IF EXISTS `payment_ledger`;
CREATE TABLE `payment_ledger` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_id` int(11) NOT NULL,
  `date_created` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `payment_id` (`order_id`),
  KEY `date_created` (`date_created`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- ----------------------------
### payment_limit
-- ----------------------------
DROP TABLE IF EXISTS `payment_limit`;
CREATE TABLE `payment_limit` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `payment_list_id` int(11) DEFAULT NULL,
  `country_id` int(11) DEFAULT NULL,
  `customer_id` int(11) DEFAULT NULL,
  `type` int(11) NOT NULL,
  `limit` decimal(12,2) NOT NULL,
  `preferred` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `payment_limit_ibfk_1` (`payment_list_id`),
  KEY `payment_limit_ibfk_2` (`country_id`),
  KEY `payment_limit_ibfk_3` (`customer_id`),
  CONSTRAINT `payment_limit_ibfk_1` FOREIGN KEY (`payment_list_id`) REFERENCES `payment_list` (`id`),
  CONSTRAINT `payment_limit_ibfk_2` FOREIGN KEY (`country_id`) REFERENCES `country` (`id`),
  CONSTRAINT `payment_limit_ibfk_3` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1867 DEFAULT CHARSET=utf8;

-- ----------------------------
### payment_list
-- ----------------------------
DROP TABLE IF EXISTS `payment_list`;
CREATE TABLE `payment_list` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(30) NOT NULL,
  `customer_name` varchar(50) NOT NULL,
  `negative_active_id` int(11) NOT NULL,
  `logo_image` varchar(32) DEFAULT NULL,
  `customer_usable` int(11) NOT NULL DEFAULT '0',
  `order_payment_method` varchar(16) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `negative_active_id` (`negative_active_id`),
  KEY `order_payment_method` (`order_payment_method`),
  CONSTRAINT `payment_list_ibfk_1` FOREIGN KEY (`negative_active_id`) REFERENCES `active` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=14 DEFAULT CHARSET=utf8;

-- ----------------------------
### paypal
-- ----------------------------
DROP TABLE IF EXISTS `paypal`;
CREATE TABLE `paypal` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `txn_type` varchar(10) DEFAULT NULL,
  `reason_code` varchar(15) DEFAULT NULL,
  `payment_type` varchar(7) DEFAULT NULL,
  `payment_status` varchar(17) DEFAULT NULL,
  `pending_reason` varchar(14) DEFAULT NULL,
  `invoice` int(11) DEFAULT NULL,
  `payer_business_name` varchar(64) DEFAULT NULL,
  `address_name` varchar(32) DEFAULT NULL,
  `address_status` varchar(11) DEFAULT NULL,
  `payer_email` varchar(96) DEFAULT NULL,
  `payer_id` varchar(32) DEFAULT NULL,
  `payer_status` varchar(10) DEFAULT NULL,
  `payment_date` datetime DEFAULT NULL,
  `payment_time_zone` varchar(4) DEFAULT NULL,
  `txn_id` varchar(17) NOT NULL DEFAULT '',
  `parent_txn_id` varchar(17) DEFAULT NULL,
  `num_cart_items` tinyint(4) unsigned DEFAULT '1',
  `mc_gross` decimal(7,2) NOT NULL DEFAULT '0.00',
  `mc_fee` decimal(7,2) NOT NULL DEFAULT '0.00',
  `payment_gross` decimal(7,2) DEFAULT NULL,
  `payment_fee` decimal(7,2) DEFAULT NULL,
  `settle_amount` decimal(7,2) DEFAULT NULL,
  `exchange_rate` decimal(4,2) DEFAULT NULL,
  `quantity` int(11) DEFAULT '0',
  `verify_sign` varchar(128) DEFAULT NULL,
  `last_modified` datetime DEFAULT NULL,
  `date_added` datetime DEFAULT NULL,
  `discount` decimal(15,4) DEFAULT NULL,
  `address_id` int(11) DEFAULT NULL,
  `site_id` int(11) NOT NULL,
  `mc_currency_id` int(11) DEFAULT NULL,
  `settle_currency_id` int(11) DEFAULT NULL,
  `protection_eligibility` varchar(64) DEFAULT NULL,
  `receipt_id` varchar(20) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `tnx_id_unique` (`txn_id`),
  KEY `paypal_ibfk_1` (`site_id`),
  KEY `paypal_ibfk_2` (`address_id`),
  KEY `paypal_ibfk_3` (`mc_currency_id`),
  KEY `paypal_ibfk_4` (`settle_currency_id`),
  KEY `invoice` (`invoice`),
  CONSTRAINT `paypal_ibfk_1` FOREIGN KEY (`site_id`) REFERENCES `site` (`id`),
  CONSTRAINT `paypal_ibfk_2` FOREIGN KEY (`address_id`) REFERENCES `address_book` (`id`),
  CONSTRAINT `paypal_ibfk_3` FOREIGN KEY (`mc_currency_id`) REFERENCES `currency` (`id`),
  CONSTRAINT `paypal_ibfk_4` FOREIGN KEY (`settle_currency_id`) REFERENCES `currency` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1487717 DEFAULT CHARSET=utf8;

-- ----------------------------
### paypal_payment_status_history
-- ----------------------------
DROP TABLE IF EXISTS `paypal_payment_status_history`;
CREATE TABLE `paypal_payment_status_history` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `paypal_id` int(11) NOT NULL DEFAULT '0',
  `payment_status` varchar(17) DEFAULT NULL,
  `pending_reason` varchar(14) DEFAULT NULL,
  `reason_code` varchar(15) DEFAULT NULL,
  `date_added` datetime NOT NULL DEFAULT '0000-00-00 00:00:00',
  PRIMARY KEY (`id`),
  KEY `paypal_payment_status_history_ibfk_1` (`paypal_id`),
  CONSTRAINT `paypal_payment_status_history_ibfk_1` FOREIGN KEY (`paypal_id`) REFERENCES `paypal` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1523302 DEFAULT CHARSET=utf8;

-- ----------------------------
### picking_log
-- ----------------------------
DROP TABLE IF EXISTS `picking_log`;
CREATE TABLE `picking_log` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_id` int(11) NOT NULL,
  `picking_id` int(11) NOT NULL,
  `date_created` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `order_id` (`order_id`),
  KEY `picking_id` (`picking_id`),
  CONSTRAINT `picking_log_ibfk_1` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1089199 DEFAULT CHARSET=utf8;

-- ----------------------------
### po_group
-- ----------------------------
DROP TABLE IF EXISTS `po_group`;
CREATE TABLE `po_group` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(60) NOT NULL,
  `level` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;

-- ----------------------------
### po_log
-- ----------------------------
DROP TABLE IF EXISTS `po_log`;
CREATE TABLE `po_log` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `po_id` int(11) NOT NULL,
  `admin_user_id` int(11) NOT NULL,
  `field` varchar(60) NOT NULL,
  `old_value` varchar(60) NOT NULL,
  `new_value` varchar(60) NOT NULL,
  `create_date` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `po_log_ibfk_1` (`admin_user_id`),
  KEY `po_log_ibfk_2` (`po_id`),
  CONSTRAINT `po_log_ibfk_1` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`),
  CONSTRAINT `po_log_ibfk_2` FOREIGN KEY (`po_id`) REFERENCES `order_purchase` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=362277 DEFAULT CHARSET=utf8;

-- ----------------------------
### po_status_history
-- ----------------------------
DROP TABLE IF EXISTS `po_status_history`;
CREATE TABLE `po_status_history` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `admin_id` int(11) DEFAULT NULL,
  `date_create` datetime NOT NULL,
  `order_purchase_id` int(11) DEFAULT NULL,
  `old_order_purchase_status_id` int(11) NOT NULL,
  `new_order_purchase_status_id` int(11) NOT NULL,
  `url` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `fk_po_status_history_1_idx` (`old_order_purchase_status_id`),
  KEY `fk_po_status_history_1_idx1` (`new_order_purchase_status_id`),
  KEY `fk_po_status_history_1_idx2` (`order_purchase_id`),
  KEY `fk_po_status_history_1_idx3` (`old_order_purchase_status_id`),
  KEY `fk_po_status_history_1_idx4` (`new_order_purchase_status_id`),
  CONSTRAINT `fk_po_status_history__new_order_purchase_status_id` FOREIGN KEY (`new_order_purchase_status_id`) REFERENCES `order_purchase_status` (`id`) ON DELETE NO ACTION ON UPDATE NO ACTION,
  CONSTRAINT `fk_po_status_history__old_order_purchase_status_id` FOREIGN KEY (`old_order_purchase_status_id`) REFERENCES `order_purchase_status` (`id`) ON DELETE NO ACTION ON UPDATE NO ACTION,
  CONSTRAINT `fk_po_status_history__order_purchase_id` FOREIGN KEY (`order_purchase_id`) REFERENCES `order_purchase` (`id`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE=InnoDB AUTO_INCREMENT=158046 DEFAULT CHARSET=utf8;

-- ----------------------------
### po_status_to_group
-- ----------------------------
DROP TABLE IF EXISTS `po_status_to_group`;
CREATE TABLE `po_status_to_group` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `po_status_id` int(11) NOT NULL,
  `po_group_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `po_status_to_group_ibfk_1` (`po_status_id`),
  KEY `po_status_to_group_ibfk_2` (`po_group_id`),
  CONSTRAINT `po_status_to_group_ibfk_1` FOREIGN KEY (`po_status_id`) REFERENCES `order_purchase_status` (`id`),
  CONSTRAINT `po_status_to_group_ibfk_2` FOREIGN KEY (`po_group_id`) REFERENCES `po_group` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;

-- ----------------------------
### po_to_w_2_w
-- ----------------------------
DROP TABLE IF EXISTS `po_to_w_2_w`;
CREATE TABLE `po_to_w_2_w` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `po_id` int(11) NOT NULL,
  `w2w_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `po_id` (`po_id`),
  KEY `w2w_id` (`w2w_id`),
  CONSTRAINT `po_to_w_2_w_ibfk_1` FOREIGN KEY (`po_id`) REFERENCES `order_purchase` (`id`),
  CONSTRAINT `po_to_w_2_w_ibfk_2` FOREIGN KEY (`w2w_id`) REFERENCES `warehouse_2_warehouse` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=133478 DEFAULT CHARSET=utf8;

-- ----------------------------
### product
-- ----------------------------
DROP TABLE IF EXISTS `product`;
CREATE TABLE `product` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `quantity` int(4) NOT NULL DEFAULT '0',
  `model` varchar(30) DEFAULT NULL,
  `manufacturer_no` varchar(12) DEFAULT NULL,
  `image` varchar(64) DEFAULT NULL,
  `price` decimal(15,4) NOT NULL DEFAULT '0.0000',
  `category_id` int(11) DEFAULT NULL,
  `date_added` datetime NOT NULL DEFAULT '0000-00-00 00:00:00',
  `last_modified` datetime DEFAULT NULL,
  `date_available` datetime DEFAULT NULL,
  `products_weight` decimal(8,3) NOT NULL DEFAULT '0.000',
  `actual_weight` decimal(8,3) DEFAULT NULL,
  `status_id` int(11) NOT NULL DEFAULT '0',
  `ordered` int(11) DEFAULT '0',
  `price_retail` decimal(15,4) NOT NULL DEFAULT '0.0000',
  `price1` decimal(15,4) NOT NULL DEFAULT '0.0000',
  `price2` decimal(15,4) NOT NULL DEFAULT '0.0000',
  `price3` decimal(15,4) NOT NULL DEFAULT '0.0000',
  `price4` decimal(15,4) NOT NULL DEFAULT '0.0000',
  `price5` decimal(15,4) NOT NULL DEFAULT '0.0000',
  `price1_qty` int(11) NOT NULL DEFAULT '0',
  `price2_qty` int(11) NOT NULL DEFAULT '0',
  `price3_qty` int(11) NOT NULL DEFAULT '0',
  `price4_qty` int(11) NOT NULL DEFAULT '0',
  `price5_qty` int(11) NOT NULL DEFAULT '0',
  `qty_blocks` int(11) NOT NULL DEFAULT '1',
  `buy_price_rmb` decimal(15,4) NOT NULL DEFAULT '0.0000',
  `sort_order` int(11) NOT NULL DEFAULT '25',
  `vendor_id` varchar(12) DEFAULT NULL,
  `ean` bigint(20) DEFAULT NULL,
  `stars` int(11) DEFAULT NULL,
  `reviews` int(11) DEFAULT NULL,
  `length` int(11) DEFAULT NULL COMMENT 'in cm',
  `width` int(11) DEFAULT NULL COMMENT 'in cm',
  `height` int(11) DEFAULT NULL COMMENT 'in cm',
  `adapter_num` int(11) DEFAULT NULL,
  `hts_code` bigint(15) DEFAULT NULL,
  `pricing_template` int(11) DEFAULT NULL,
  `lead_time_days` int(11) DEFAULT NULL,
  `owner_description_id` int(11) DEFAULT NULL,
  `owner_media_id` int(11) DEFAULT NULL,
  `owner_manager_id` int(11) DEFAULT NULL,
  `date_back` datetime DEFAULT NULL,
  `avg_price_rmb` decimal(15,4) DEFAULT NULL,
  `star_delivery` int(11) DEFAULT NULL,
  `star_value` int(11) DEFAULT NULL,
  `star_packing` int(11) DEFAULT NULL,
  `star_ease` int(11) DEFAULT NULL,
  `star_function` int(11) DEFAULT NULL,
  `owner_sourcing_id` int(11) DEFAULT NULL,
  `included_shipping_value` decimal(15,4) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `idx_products_date_added` (`date_added`),
  KEY `sort_order` (`sort_order`),
  KEY `products_status` (`status_id`),
  KEY `products_model` (`model`),
  KEY `products_ordered` (`ordered`),
  KEY `products_last_modified` (`last_modified`),
  KEY `index_products_ean` (`ean`),
  KEY `reviews` (`reviews`),
  KEY `category_id` (`category_id`),
  KEY `owner_description` (`owner_description_id`),
  KEY `owner_media` (`owner_media_id`),
  KEY `owner_manager` (`owner_manager_id`),
  KEY `owner_sourcing_id` (`owner_sourcing_id`),
  CONSTRAINT `product_ibfk_1` FOREIGN KEY (`category_id`) REFERENCES `category` (`id`),
  CONSTRAINT `product_ibfk_2` FOREIGN KEY (`owner_description_id`) REFERENCES `admin_user` (`id`),
  CONSTRAINT `product_ibfk_3` FOREIGN KEY (`owner_media_id`) REFERENCES `admin_user` (`id`),
  CONSTRAINT `product_ibfk_4` FOREIGN KEY (`owner_manager_id`) REFERENCES `admin_user` (`id`),
  CONSTRAINT `product_ibfk_5` FOREIGN KEY (`owner_sourcing_id`) REFERENCES `admin_user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=23484 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_360_image
-- ----------------------------
DROP TABLE IF EXISTS `product_360_image`;
CREATE TABLE `product_360_image` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL,
  `image` varchar(64) NOT NULL,
  `sort_order` int(11) NOT NULL,
  `date_uploaded` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `product_id` (`product_id`),
  KEY `sort_order` (`sort_order`),
  CONSTRAINT `product_360_image_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=192377 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_accessories
-- ----------------------------
DROP TABLE IF EXISTS `product_accessories`;
CREATE TABLE `product_accessories` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL,
  `accessorie_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `product_id` (`product_id`,`accessorie_id`),
  KEY `product_id_2` (`product_id`),
  KEY `accessorie_id` (`accessorie_id`),
  CONSTRAINT `product_accessories_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
  CONSTRAINT `product_accessories_ibfk_2` FOREIGN KEY (`accessorie_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=19197 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_also
-- ----------------------------
DROP TABLE IF EXISTS `product_also`;
CREATE TABLE `product_also` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL,
  `also_id` int(11) NOT NULL,
  `percentage` int(11) NOT NULL,
  `stars` int(11) DEFAULT NULL,
  `reviews` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `product_id` (`product_id`),
  KEY `also_id` (`also_id`),
  CONSTRAINT `product_also_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
  CONSTRAINT `product_also_ibfk_2` FOREIGN KEY (`also_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=25371596 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_banned_country
-- ----------------------------
DROP TABLE IF EXISTS `product_banned_country`;
CREATE TABLE `product_banned_country` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL,
  `country_id` int(11) DEFAULT NULL,
  `courier_id` int(11) DEFAULT NULL,
  `exception_customer_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `product_id_2` (`product_id`),
  KEY `country_id` (`country_id`),
  KEY `courier_id` (`courier_id`),
  KEY `exception_customer_id` (`exception_customer_id`),
  CONSTRAINT `product_banned_country_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
  CONSTRAINT `product_banned_country_ibfk_2` FOREIGN KEY (`country_id`) REFERENCES `country` (`id`),
  CONSTRAINT `product_banned_country_ibfk_3` FOREIGN KEY (`courier_id`) REFERENCES `courier` (`id`),
  CONSTRAINT `product_banned_country_ibfk_4` FOREIGN KEY (`exception_customer_id`) REFERENCES `customer` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=7546 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_description
-- ----------------------------
DROP TABLE IF EXISTS `product_description`;
CREATE TABLE `product_description` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL,
  `language_id` int(11) NOT NULL DEFAULT '1',
  `name` varchar(128) DEFAULT NULL,
  `name_short` varchar(45) DEFAULT NULL,
  `description` varchar(5000) DEFAULT NULL,
  `description_short` varchar(200) DEFAULT NULL,
  `description2` varchar(10000) DEFAULT NULL,
  `alternative` varchar(60) DEFAULT NULL,
  `url` varchar(255) DEFAULT NULL,
  `viewed` int(5) DEFAULT '0',
  `youtube` varchar(13) DEFAULT NULL,
  `youtube2` varchar(13) DEFAULT NULL,
  `meta_keywords` varchar(255) DEFAULT NULL,
  `meta_description` varchar(160) DEFAULT NULL,
  `meta_title` varchar(255) DEFAULT NULL,
  `status_comment_public` varchar(1000) DEFAULT NULL,
  `status_comment_admin` varchar(1000) DEFAULT NULL,
  `description_dropship` varchar(1000) DEFAULT NULL,
  `description_short2` varchar(250) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `products_name` (`name`),
  KEY `products_id` (`product_id`),
  KEY `language_id` (`language_id`),
  CONSTRAINT `product_description_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=22501 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_description_wh
-- ----------------------------
DROP TABLE IF EXISTS `product_description_wh`;
CREATE TABLE `product_description_wh` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL,
  `zh_name` varchar(200) DEFAULT NULL,
  `description` text,
  PRIMARY KEY (`id`),
  KEY `product_id` (`product_id`),
  CONSTRAINT `product_description_wh_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=16259 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_index
-- ----------------------------
DROP TABLE IF EXISTS `product_index`;
CREATE TABLE `product_index` (
  `id` bigint(20) unsigned NOT NULL COMMENT 'product.id',
  `weight` int(10) unsigned NOT NULL,
  `query` varchar(3072) NOT NULL,
  KEY `query` (`query`(255))
) ENGINE=InnoDB DEFAULT CHARSET=utf8 CONNECTION='sphinx://127.0.0.1:9312/p_delta:p_index';

-- ----------------------------
### product_index_cache
-- ----------------------------
DROP TABLE IF EXISTS `product_index_cache`;
CREATE TABLE `product_index_cache` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL,
  `name` text,
  `description` text,
  `description2` text,
  `meta_description` text,
  `meta_keywords` text,
  `model` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `product_id` (`product_id`)
) ENGINE=InnoDB AUTO_INCREMENT=307357 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_kpi
-- ----------------------------
DROP TABLE IF EXISTS `product_kpi`;
CREATE TABLE `product_kpi` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `field_type` int(11) NOT NULL,
  `date_created` datetime NOT NULL,
  `value` int(11) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=25078 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_log
-- ----------------------------
DROP TABLE IF EXISTS `product_log`;
CREATE TABLE `product_log` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL,
  `admin_user_id` int(11) NOT NULL,
  `status_id` int(11) NOT NULL,
  `field` varchar(60) NOT NULL,
  `old_value` varchar(60) NOT NULL,
  `new_value` varchar(60) NOT NULL,
  `create_date` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `product_log_ibfk_1` (`product_id`),
  KEY `product_log_ibfk_2` (`admin_user_id`),
  CONSTRAINT `product_log_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
  CONSTRAINT `product_log_ibfk_2` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2147431 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_price
-- ----------------------------
DROP TABLE IF EXISTS `product_price`;
CREATE TABLE `product_price` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL,
  `currency_id` int(11) NOT NULL,
  `special_id` int(11) DEFAULT NULL,
  `price_retail` decimal(15,4) DEFAULT NULL,
  `price` decimal(15,4) NOT NULL,
  `price1` decimal(15,4) DEFAULT NULL,
  `price2` decimal(15,4) DEFAULT NULL,
  `price3` decimal(15,4) DEFAULT NULL,
  `price4` decimal(15,4) DEFAULT NULL,
  `price5` decimal(15,4) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `product_id` (`product_id`),
  KEY `currency_id` (`currency_id`),
  KEY `special_id` (`special_id`),
  CONSTRAINT `product_price_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
  CONSTRAINT `product_price_ibfk_2` FOREIGN KEY (`currency_id`) REFERENCES `currency` (`id`),
  CONSTRAINT `product_price_ibfk_3` FOREIGN KEY (`special_id`) REFERENCES `special` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=48439 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_see_also
-- ----------------------------
DROP TABLE IF EXISTS `product_see_also`;
CREATE TABLE `product_see_also` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL,
  `see_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `product_id` (`product_id`,`see_id`),
  KEY `product_id_2` (`product_id`),
  KEY `see_id` (`see_id`),
  CONSTRAINT `product_see_also_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
  CONSTRAINT `product_see_also_ibfk_2` FOREIGN KEY (`see_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=108779 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_to_battery
-- ----------------------------
DROP TABLE IF EXISTS `product_to_battery`;
CREATE TABLE `product_to_battery` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL,
  `battery_id` int(11) NOT NULL,
  `quantity` int(11) DEFAULT NULL,
  `replaceable` int(11) DEFAULT NULL,
  `included` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `product_id` (`product_id`),
  KEY `battery_id` (`battery_id`),
  KEY `product_to_battery_active_replaceable_FK` (`replaceable`),
  KEY `product_to_battery_active_included_FK` (`included`),
  CONSTRAINT `product_to_battery_active_included_FK` FOREIGN KEY (`included`) REFERENCES `active` (`id`),
  CONSTRAINT `product_to_battery_active_replaceable_FK` FOREIGN KEY (`replaceable`) REFERENCES `active` (`id`),
  CONSTRAINT `product_to_battery_battery_FK` FOREIGN KEY (`battery_id`) REFERENCES `battery` (`id`),
  CONSTRAINT `product_to_battery_product_FK` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1972 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_to_category
-- ----------------------------
DROP TABLE IF EXISTS `product_to_category`;
CREATE TABLE `product_to_category` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL DEFAULT '0',
  `category_id` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`),
  KEY `products_id` (`product_id`),
  KEY `categories_id` (`category_id`),
  CONSTRAINT `product_to_category_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
  CONSTRAINT `product_to_category_ibfk_2` FOREIGN KEY (`category_id`) REFERENCES `category` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=33834 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_to_flag
-- ----------------------------
DROP TABLE IF EXISTS `product_to_flag`;
CREATE TABLE `product_to_flag` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL,
  `flag_id` int(11) NOT NULL,
  `admin_user_id` int(11) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `product_to_flag_ibfk_1` (`product_id`),
  KEY `product_to_flag_ibfk_2` (`flag_id`),
  KEY `product_to_flag_ibfk_3` (`admin_user_id`),
  CONSTRAINT `product_to_flag_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
  CONSTRAINT `product_to_flag_ibfk_2` FOREIGN KEY (`flag_id`) REFERENCES `flag` (`id`),
  CONSTRAINT `product_to_flag_ibfk_3` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=139723 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_to_vendor
-- ----------------------------
DROP TABLE IF EXISTS `product_to_vendor`;
CREATE TABLE `product_to_vendor` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL,
  `vendor_id` int(11) NOT NULL,
  `supplier_code` varchar(60) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `product_id` (`product_id`),
  KEY `vendor_id` (`vendor_id`),
  CONSTRAINT `product_to_vendor_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
  CONSTRAINT `product_to_vendor_ibfk_2` FOREIGN KEY (`vendor_id`) REFERENCES `vendor` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=24905 DEFAULT CHARSET=utf8;

-- ----------------------------
### product_wh
-- ----------------------------
DROP TABLE IF EXISTS `product_wh`;
CREATE TABLE `product_wh` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `warehouse_id` int(11) NOT NULL DEFAULT '1',
  `product_id` int(11) NOT NULL,
  `quantity_minimum` int(11) NOT NULL,
  `quantity_order` int(11) NOT NULL,
  `quantity_order_supplier` int(11) NOT NULL DEFAULT '1',
  `avg_sales_per_day_override` decimal(15,4) DEFAULT NULL,
  `difficulty` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `product_id_2` (`product_id`),
  UNIQUE KEY `product_id_3` (`product_id`),
  KEY `warehouse_id` (`warehouse_id`),
  KEY `product_id` (`product_id`),
  CONSTRAINT `product_wh_ibfk_1` FOREIGN KEY (`warehouse_id`) REFERENCES `warehouse` (`id`),
  CONSTRAINT `product_wh_ibfk_2` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=26625 DEFAULT CHARSET=utf8;

-- ----------------------------
### redirect
-- ----------------------------
DROP TABLE IF EXISTS `redirect`;
CREATE TABLE `redirect` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `site_id` int(11) NOT NULL DEFAULT '1',
  `redirect_from` varchar(255) NOT NULL,
  `redirect_to` varchar(255) NOT NULL,
  `tracking` int(11) NOT NULL DEFAULT '1',
  `comments` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `rfrom` (`redirect_from`,`site_id`) USING BTREE,
  KEY `rto` (`redirect_to`),
  KEY `active_id` (`tracking`),
  KEY `site_id` (`site_id`),
  CONSTRAINT `redirect_ibfk_1` FOREIGN KEY (`site_id`) REFERENCES `site` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=71918 DEFAULT CHARSET=utf8;

-- ----------------------------
### redirect_index
-- ----------------------------
DROP TABLE IF EXISTS `redirect_index`;
CREATE TABLE `redirect_index` (
  `id` int(10) unsigned NOT NULL COMMENT 'redirect.id',
  `weight` int(10) unsigned NOT NULL,
  `query` varchar(3072) NOT NULL,
  KEY `query` (`query`(255))
) ENGINE=InnoDB DEFAULT CHARSET=utf8 CONNECTION='sphinx://127.0.0.1:9312/redirect_index';

-- ----------------------------
### referrer_site
-- ----------------------------
DROP TABLE IF EXISTS `referrer_site`;
CREATE TABLE `referrer_site` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(30) NOT NULL,
  `define` varchar(40) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=192830 DEFAULT CHARSET=utf8;

-- ----------------------------
### referrer_url
-- ----------------------------
DROP TABLE IF EXISTS `referrer_url`;
CREATE TABLE `referrer_url` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `url` text NOT NULL,
  `md5_hash` varchar(32) NOT NULL,
  `referrer_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `referrer_id` (`referrer_id`),
  KEY `md5_hash` (`md5_hash`),
  CONSTRAINT `referrer_url_ibfk_1` FOREIGN KEY (`referrer_id`) REFERENCES `referrer_site` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2528 DEFAULT CHARSET=utf8;

-- ----------------------------
### review
-- ----------------------------
DROP TABLE IF EXISTS `review`;
CREATE TABLE `review` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `site_id` int(11) NOT NULL DEFAULT '1',
  `product_id` int(11) NOT NULL DEFAULT '0',
  `customer_id` int(11) DEFAULT NULL,
  `customer_name` varchar(64) DEFAULT NULL,
  `reviews_rating` int(1) DEFAULT NULL,
  `date_added` datetime DEFAULT NULL,
  `last_modified` datetime DEFAULT NULL,
  `review_status_id` int(10) unsigned NOT NULL DEFAULT '1',
  `star_delivery` int(11) DEFAULT NULL,
  `star_value` int(11) DEFAULT NULL,
  `star_packing` int(11) DEFAULT NULL,
  `star_ease` int(11) DEFAULT NULL,
  `star_function` int(11) DEFAULT NULL,
  `version` int(11) NOT NULL DEFAULT '1',
  `user_country_id` int(11) DEFAULT NULL,
  `verified_buyer` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `products_id` (`product_id`),
  KEY `customers_id` (`customer_id`),
  KEY `site_id` (`site_id`),
  KEY `review_status_id` (`review_status_id`),
  KEY `user_country_id` (`user_country_id`),
  CONSTRAINT `review_ibfk_1` FOREIGN KEY (`site_id`) REFERENCES `site` (`id`),
  CONSTRAINT `review_ibfk_2` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
  CONSTRAINT `review_ibfk_3` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
  CONSTRAINT `review_ibfk_4` FOREIGN KEY (`review_status_id`) REFERENCES `review_status` (`id`),
  CONSTRAINT `review_ibfk_5` FOREIGN KEY (`user_country_id`) REFERENCES `country` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=63714 DEFAULT CHARSET=utf8;

-- ----------------------------
### review_description
-- ----------------------------
DROP TABLE IF EXISTS `review_description`;
CREATE TABLE `review_description` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `review_id` int(11) NOT NULL DEFAULT '0',
  `title` varchar(100) DEFAULT NULL,
  `text` text,
  `good` text,
  `bad` text,
  PRIMARY KEY (`id`),
  KEY `reviews_id` (`review_id`),
  CONSTRAINT `review_description_ibfk_1` FOREIGN KEY (`review_id`) REFERENCES `review` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=38120 DEFAULT CHARSET=utf8;

-- ----------------------------
### review_product_notify
-- ----------------------------
DROP TABLE IF EXISTS `review_product_notify`;
CREATE TABLE `review_product_notify` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `customer_id` int(11) NOT NULL,
  `product_id` int(11) NOT NULL,
  `date_create` date NOT NULL,
  PRIMARY KEY (`id`),
  KEY `customer_id` (`customer_id`),
  KEY `product_id` (`product_id`),
  CONSTRAINT `review_product_notify_ibfk_1` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
  CONSTRAINT `review_product_notify_ibfk_2` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- ----------------------------
### review_status
-- ----------------------------
DROP TABLE IF EXISTS `review_status`;
CREATE TABLE `review_status` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(32) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma
-- ----------------------------
DROP TABLE IF EXISTS `rma`;
CREATE TABLE `rma` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `cs_ticket` varchar(13) NOT NULL,
  `cs_ticket_parts` int(11) DEFAULT '0',
  `rma_date` datetime NOT NULL,
  `rma_status_id` int(11) NOT NULL,
  `customer_id` int(11) NOT NULL,
  `customer_class_id` int(11) DEFAULT NULL,
  `special_vip` int(11) DEFAULT '0',
  `contact_name` varchar(64) DEFAULT NULL,
  `contact_email` varchar(96) DEFAULT NULL,
  `sent_country_id` int(11) DEFAULT NULL,
  `received_office_id` int(11) DEFAULT NULL,
  `return_address_id` int(11) DEFAULT NULL,
  `use_paypal_id` int(11) DEFAULT NULL,
  `authorized_id` int(11) DEFAULT NULL,
  `assigned_user_id` int(11) NOT NULL,
  `resolve_as_id` int(11) DEFAULT NULL,
  `leave_as_id` int(11) NOT NULL,
  `repaired_still_faulty_flag` int(11) DEFAULT '0',
  `repaired_still_faulty_ref` varchar(13) DEFAULT NULL,
  `black_case` int(11) DEFAULT '0',
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `rma_fk_rma_status_id` (`rma_status_id`),
  KEY `rma_fk_customer_id` (`customer_id`),
  KEY `rma_fk_customer_class_id` (`customer_class_id`),
  KEY `rma_fk_sent_country_id` (`sent_country_id`),
  KEY `rma_fk_received_office_id` (`received_office_id`),
  KEY `rma_fk_return_address_id` (`return_address_id`),
  KEY `rma_fk_use_paypal_id` (`use_paypal_id`),
  KEY `rma_fk_authorized_id` (`authorized_id`),
  KEY `rma_fk_assigned_user_id` (`assigned_user_id`),
  KEY `rma_fk_resolve_as_id` (`resolve_as_id`),
  KEY `rma_fk_leave_as_id` (`leave_as_id`),
  CONSTRAINT `rma_fk_assigned_user_id` FOREIGN KEY (`assigned_user_id`) REFERENCES `admin_user` (`id`),
  CONSTRAINT `rma_fk_authorized_id` FOREIGN KEY (`authorized_id`) REFERENCES `active` (`id`),
  CONSTRAINT `rma_fk_customer_class_id` FOREIGN KEY (`customer_class_id`) REFERENCES `rma_customer_class` (`id`),
  CONSTRAINT `rma_fk_customer_id` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
  CONSTRAINT `rma_fk_leave_as_id` FOREIGN KEY (`leave_as_id`) REFERENCES `rma_lifecycle` (`id`),
  CONSTRAINT `rma_fk_received_office_id` FOREIGN KEY (`received_office_id`) REFERENCES `rma_received_office` (`id`),
  CONSTRAINT `rma_fk_resolve_as_id` FOREIGN KEY (`resolve_as_id`) REFERENCES `rma_solution` (`id`),
  CONSTRAINT `rma_fk_return_address_id` FOREIGN KEY (`return_address_id`) REFERENCES `address_book` (`id`),
  CONSTRAINT `rma_fk_rma_status_id` FOREIGN KEY (`rma_status_id`) REFERENCES `rma_status` (`id`),
  CONSTRAINT `rma_fk_sent_country_id` FOREIGN KEY (`sent_country_id`) REFERENCES `country` (`id`),
  CONSTRAINT `rma_fk_use_paypal_id` FOREIGN KEY (`use_paypal_id`) REFERENCES `active` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=19894 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_cs_consult_desc
-- ----------------------------
DROP TABLE IF EXISTS `rma_cs_consult_desc`;
CREATE TABLE `rma_cs_consult_desc` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_customer_class
-- ----------------------------
DROP TABLE IF EXISTS `rma_customer_class`;
CREATE TABLE `rma_customer_class` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_faulty_category
-- ----------------------------
DROP TABLE IF EXISTS `rma_faulty_category`;
CREATE TABLE `rma_faulty_category` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=20 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_faulty_reason
-- ----------------------------
DROP TABLE IF EXISTS `rma_faulty_reason`;
CREATE TABLE `rma_faulty_reason` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `rma_faulty_category_id` int(11) NOT NULL,
  `name` varchar(255) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `rma_faulty_reason_fk_rma_faulty_category_id` (`rma_faulty_category_id`),
  CONSTRAINT `rma_faulty_reason_fk_rma_faulty_category_id` FOREIGN KEY (`rma_faulty_category_id`) REFERENCES `rma_faulty_category` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=142 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_faulty_selection
-- ----------------------------
DROP TABLE IF EXISTS `rma_faulty_selection`;
CREATE TABLE `rma_faulty_selection` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `rma_id` int(11) NOT NULL,
  `rma_product_id` int(11) NOT NULL,
  `faulty_selected_phase` int(11) NOT NULL,
  `rma_faulty_reason_id` int(11) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `rma_faulty_selection_fk_rma_id` (`rma_id`),
  KEY `rma_faulty_selection_fk_rma_product_id` (`rma_product_id`),
  KEY `rma_faulty_selection_fk_rma_faulty_reason_id` (`rma_faulty_reason_id`),
  CONSTRAINT `rma_faulty_selection_fk_rma_faulty_reason_id` FOREIGN KEY (`rma_faulty_reason_id`) REFERENCES `rma_faulty_reason` (`id`),
  CONSTRAINT `rma_faulty_selection_fk_rma_id` FOREIGN KEY (`rma_id`) REFERENCES `rma` (`id`),
  CONSTRAINT `rma_faulty_selection_fk_rma_product_id` FOREIGN KEY (`rma_product_id`) REFERENCES `rma_product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=24528 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_lifecycle
-- ----------------------------
DROP TABLE IF EXISTS `rma_lifecycle`;
CREATE TABLE `rma_lifecycle` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_log
-- ----------------------------
DROP TABLE IF EXISTS `rma_log`;
CREATE TABLE `rma_log` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `rma_id` int(11) NOT NULL,
  `rma_product_id` int(11) DEFAULT NULL,
  `admin_user_id` int(11) DEFAULT NULL,
  `field` varchar(60) DEFAULT NULL,
  `old_value` varchar(60) DEFAULT NULL,
  `new_value` varchar(60) DEFAULT NULL,
  `create_date` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `rma_log_fk_rma_id` (`rma_id`),
  KEY `rma_log_fk_rma_product_id` (`rma_product_id`),
  KEY `rma_log_fk_admin_user_id` (`admin_user_id`),
  CONSTRAINT `rma_log_fk_admin_user_id` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`),
  CONSTRAINT `rma_log_fk_rma_id` FOREIGN KEY (`rma_id`) REFERENCES `rma` (`id`),
  CONSTRAINT `rma_log_fk_rma_product_id` FOREIGN KEY (`rma_product_id`) REFERENCES `rma_product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=380829 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_product
-- ----------------------------
DROP TABLE IF EXISTS `rma_product`;
CREATE TABLE `rma_product` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `rma_id` int(11) NOT NULL,
  `order_id` int(11) DEFAULT NULL,
  `product_id` int(11) DEFAULT NULL,
  `warranty_id` int(11) DEFAULT NULL,
  `comments` varchar(1000) DEFAULT NULL,
  `customer_faulty_reason_id` int(11) DEFAULT NULL,
  `customer_remark` varchar(1000) DEFAULT NULL,
  `product_rec_date` datetime DEFAULT NULL,
  `accessories` varchar(1000) DEFAULT NULL,
  `less_accessories_from_customer` int(11) DEFAULT '0',
  `qc_tested_date` datetime DEFAULT NULL,
  `qc_tested_faulty_reason_id` int(11) DEFAULT NULL,
  `qc_remark` varchar(1000) DEFAULT NULL,
  `cs_consult_date1` datetime DEFAULT NULL,
  `cs_consult_date2` datetime DEFAULT NULL,
  `cs_consult_desc_id` int(11) DEFAULT NULL,
  `happy_stock_checked` int(11) DEFAULT '0',
  `total_rec_currency_id` int(11) DEFAULT NULL,
  `total_rec_amount` decimal(15,4) DEFAULT '0.0000',
  `ok_to_repair` int(11) DEFAULT NULL,
  `repair_mode_id` int(11) DEFAULT NULL,
  `repair_currency_id` int(11) DEFAULT NULL,
  `repair_cost` decimal(15,4) DEFAULT '0.0000',
  `service_currency_id` int(11) DEFAULT NULL,
  `service_cost` decimal(15,4) DEFAULT '0.0000',
  `charge_reason` varchar(1000) DEFAULT NULL,
  `rma_product_quality_id` int(11) DEFAULT NULL,
  `repair_returned_date1` datetime DEFAULT NULL,
  `repair_confirmed_date1` datetime DEFAULT NULL,
  `repair_received_date1` datetime DEFAULT NULL,
  `repair_returned_date2` datetime DEFAULT NULL,
  `repair_confirmed_date2` datetime DEFAULT NULL,
  `repair_received_date2` datetime DEFAULT NULL,
  `qc_tested_ok_date` datetime DEFAULT NULL,
  `repaired_still_faulty` int(11) DEFAULT '0',
  `less_accessories_from_supplier` int(11) DEFAULT '0',
  `confirmed_no_repair` int(11) DEFAULT '0',
  `payment_deducted` int(11) DEFAULT '0',
  `supplier_faulty_reason_id` int(11) DEFAULT NULL,
  `supplier_remark` varchar(1000) DEFAULT NULL,
  `in_stock_date` datetime DEFAULT NULL,
  `in_stock_checked` int(11) DEFAULT '0',
  `solved_date` datetime DEFAULT NULL,
  `resolve_as_id` int(11) DEFAULT NULL,
  `refund_reason_id` int(11) DEFAULT NULL,
  `refund_currency_id` int(11) DEFAULT NULL,
  `refund_amount` decimal(15,4) DEFAULT NULL,
  `request_type_id` int(11) DEFAULT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `rma_product_fk_rma_id` (`rma_id`),
  KEY `rma_product_fk_order_id` (`order_id`),
  KEY `rma_product_fk_product_id` (`product_id`),
  KEY `rma_product_fk_warranty_id` (`warranty_id`),
  KEY `rma_product_fk_customer_faulty_reason_id` (`customer_faulty_reason_id`),
  KEY `rma_product_fk_qc_tested_faulty_reason_id` (`qc_tested_faulty_reason_id`),
  KEY `rma_product_fk_repair_mode_id` (`repair_mode_id`),
  KEY `rma_product_fk_cs_consult_desc_id` (`cs_consult_desc_id`),
  KEY `rma_product_fk_repair_currency_id` (`repair_currency_id`),
  KEY `rma_product_fk_service_currency_id` (`service_currency_id`),
  KEY `rma_product_fk_total_rec_currency_id` (`total_rec_currency_id`),
  KEY `rma_product_fk_supplier_faulty_reason_id` (`supplier_faulty_reason_id`),
  KEY `rma_product_fk_resolve_as_id` (`resolve_as_id`),
  KEY `rma_product_fk_refund_reason_id` (`refund_reason_id`),
  KEY `rma_product_fk_refund_currency_id` (`refund_currency_id`),
  KEY `rma_product_fk_rma_product_quality` (`rma_product_quality_id`),
  KEY `request_type_id` (`request_type_id`),
  CONSTRAINT `rma_product_fk_cs_consult_desc_id` FOREIGN KEY (`cs_consult_desc_id`) REFERENCES `rma_cs_consult_desc` (`id`),
  CONSTRAINT `rma_product_fk_customer_faulty_reason_id` FOREIGN KEY (`customer_faulty_reason_id`) REFERENCES `rma_faulty_reason` (`id`),
  CONSTRAINT `rma_product_fk_order_id` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`),
  CONSTRAINT `rma_product_fk_product_id` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
  CONSTRAINT `rma_product_fk_qc_tested_faulty_reason_id` FOREIGN KEY (`qc_tested_faulty_reason_id`) REFERENCES `rma_faulty_reason` (`id`),
  CONSTRAINT `rma_product_fk_refund_currency_id` FOREIGN KEY (`refund_currency_id`) REFERENCES `currency` (`id`),
  CONSTRAINT `rma_product_fk_refund_reason_id` FOREIGN KEY (`refund_reason_id`) REFERENCES `rma_refund_reason` (`id`),
  CONSTRAINT `rma_product_fk_repair_currency_id` FOREIGN KEY (`repair_currency_id`) REFERENCES `currency` (`id`),
  CONSTRAINT `rma_product_fk_repair_mode_id` FOREIGN KEY (`repair_mode_id`) REFERENCES `rma_repair_mode` (`id`),
  CONSTRAINT `rma_product_fk_request_type_id` FOREIGN KEY (`request_type_id`) REFERENCES `rma_request_type` (`id`),
  CONSTRAINT `rma_product_fk_resolve_as_id` FOREIGN KEY (`resolve_as_id`) REFERENCES `rma_solution` (`id`),
  CONSTRAINT `rma_product_fk_rma_id` FOREIGN KEY (`rma_id`) REFERENCES `rma` (`id`),
  CONSTRAINT `rma_product_fk_rma_product_quality` FOREIGN KEY (`rma_product_quality_id`) REFERENCES `rma_product_quality` (`id`),
  CONSTRAINT `rma_product_fk_service_currency_id` FOREIGN KEY (`service_currency_id`) REFERENCES `currency` (`id`),
  CONSTRAINT `rma_product_fk_supplier_faulty_reason_id` FOREIGN KEY (`supplier_faulty_reason_id`) REFERENCES `rma_faulty_reason` (`id`),
  CONSTRAINT `rma_product_fk_total_rec_currency_id` FOREIGN KEY (`total_rec_currency_id`) REFERENCES `currency` (`id`),
  CONSTRAINT `rma_product_fk_warranty_id` FOREIGN KEY (`warranty_id`) REFERENCES `rma_warranty` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=19701 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_product_quality
-- ----------------------------
DROP TABLE IF EXISTS `rma_product_quality`;
CREATE TABLE `rma_product_quality` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(64) COLLATE utf8_unicode_ci NOT NULL,
  `active` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `FK__active` (`active`),
  CONSTRAINT `FK__active` FOREIGN KEY (`active`) REFERENCES `active` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;

-- ----------------------------
### rma_received_office
-- ----------------------------
DROP TABLE IF EXISTS `rma_received_office`;
CREATE TABLE `rma_received_office` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_refund_reason
-- ----------------------------
DROP TABLE IF EXISTS `rma_refund_reason`;
CREATE TABLE `rma_refund_reason` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=30 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_repair_mode
-- ----------------------------
DROP TABLE IF EXISTS `rma_repair_mode`;
CREATE TABLE `rma_repair_mode` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_request_type
-- ----------------------------
DROP TABLE IF EXISTS `rma_request_type`;
CREATE TABLE `rma_request_type` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_solution
-- ----------------------------
DROP TABLE IF EXISTS `rma_solution`;
CREATE TABLE `rma_solution` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=32 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_solve_as
-- ----------------------------
DROP TABLE IF EXISTS `rma_solve_as`;
CREATE TABLE `rma_solve_as` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_status
-- ----------------------------
DROP TABLE IF EXISTS `rma_status`;
CREATE TABLE `rma_status` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=28 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_uploads
-- ----------------------------
DROP TABLE IF EXISTS `rma_uploads`;
CREATE TABLE `rma_uploads` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `rma_id` int(11) NOT NULL,
  `admin_user_id` int(11) NOT NULL,
  `user_file_name` varchar(128) NOT NULL,
  `file_name` varchar(16) NOT NULL,
  `date_create` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `rma_id` (`rma_id`),
  KEY `admin_user_id` (`admin_user_id`),
  KEY `file_name` (`file_name`),
  CONSTRAINT `rma_uploads_ibfk_1` FOREIGN KEY (`rma_id`) REFERENCES `rma` (`id`),
  CONSTRAINT `rma_uploads_ibfk_2` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=785 DEFAULT CHARSET=utf8;

-- ----------------------------
### rma_warranty
-- ----------------------------
DROP TABLE IF EXISTS `rma_warranty`;
CREATE TABLE `rma_warranty` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=16 DEFAULT CHARSET=utf8;

-- ----------------------------
### search_helper
-- ----------------------------
DROP TABLE IF EXISTS `search_helper`;
CREATE TABLE `search_helper` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(40) NOT NULL,
  `regex` varchar(100) NOT NULL,
  `replace` varchar(100) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;

-- ----------------------------
### site
-- ----------------------------
DROP TABLE IF EXISTS `site`;
CREATE TABLE `site` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `hostname` varchar(100) NOT NULL,
  `rss_path` varchar(100) DEFAULT NULL,
  `theme` varchar(100) NOT NULL,
  `paypal_name` varchar(96) NOT NULL,
  `paypal_email` varchar(96) NOT NULL,
  `paypal_id` varchar(32) NOT NULL,
  `aliasfor` int(11) DEFAULT NULL,
  `active_id` int(11) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=16 DEFAULT CHARSET=utf8;

-- ----------------------------
### site_to_address_book
-- ----------------------------
DROP TABLE IF EXISTS `site_to_address_book`;
CREATE TABLE `site_to_address_book` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `site_id` int(11) NOT NULL,
  `courier_id` int(11) DEFAULT NULL,
  `address_book_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `site_id` (`site_id`),
  KEY `courier_id` (`courier_id`),
  KEY `address_book_id` (`address_book_id`),
  CONSTRAINT `site_to_address_book_ibfk_1` FOREIGN KEY (`site_id`) REFERENCES `site` (`id`),
  CONSTRAINT `site_to_address_book_ibfk_2` FOREIGN KEY (`courier_id`) REFERENCES `courier` (`id`),
  CONSTRAINT `site_to_address_book_ibfk_3` FOREIGN KEY (`address_book_id`) REFERENCES `address_book` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;

-- ----------------------------
### skrill
-- ----------------------------
DROP TABLE IF EXISTS `skrill`;
CREATE TABLE `skrill` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `transaction_id` varchar(20) NOT NULL,
  `pay_from_email` varchar(64) NOT NULL,
  `amount` decimal(12,4) NOT NULL,
  `currency` varchar(3) NOT NULL,
  `mb_amount` decimal(12,4) NOT NULL,
  `mb_curency` varchar(3) NOT NULL,
  `status` tinyint(4) NOT NULL,
  `md5sig` varchar(32) NOT NULL,
  `time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `failed_reason_code` tinyint(3) unsigned DEFAULT NULL,
  `payment_type` varchar(32) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `transaction_id` (`transaction_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- ----------------------------
### socket_list
-- ----------------------------
DROP TABLE IF EXISTS `socket_list`;
CREATE TABLE `socket_list` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `socket_char` varchar(1) NOT NULL,
  `label` varchar(100) NOT NULL,
  `picture_name` varchar(255) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `socket_char` (`socket_char`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;

-- ----------------------------
### special
-- ----------------------------
DROP TABLE IF EXISTS `special`;
CREATE TABLE `special` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL DEFAULT '0',
  `new_products_price` decimal(15,4) NOT NULL DEFAULT '0.0000',
  `date_added` datetime DEFAULT NULL,
  `last_modified` datetime DEFAULT NULL,
  `expires_date` datetime DEFAULT NULL,
  `status` int(1) NOT NULL DEFAULT '1',
  `customer_status_id` int(11) DEFAULT NULL,
  `customer_id` int(11) DEFAULT NULL,
  `admin_user_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `products_id` (`product_id`),
  KEY `expires_date` (`expires_date`),
  KEY `status` (`status`),
  KEY `customer_id` (`customer_id`),
  KEY `customer_status_id` (`customer_status_id`),
  KEY `special_ibfk_4` (`admin_user_id`),
  CONSTRAINT `special_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
  CONSTRAINT `special_ibfk_2` FOREIGN KEY (`customer_status_id`) REFERENCES `customer_status` (`id`),
  CONSTRAINT `special_ibfk_3` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
  CONSTRAINT `special_ibfk_4` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=21652 DEFAULT CHARSET=utf8;

-- ----------------------------
### url
-- ----------------------------
DROP TABLE IF EXISTS `url`;
CREATE TABLE `url` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `hash` bigint(20) NOT NULL,
  `url` varchar(255) NOT NULL,
  `site_id` int(11) NOT NULL DEFAULT '1',
  `category_id` int(11) DEFAULT NULL,
  `page_id` int(11) DEFAULT NULL,
  `product_id` int(11) DEFAULT NULL,
  `page_template` varchar(50) DEFAULT NULL,
  `meta_keywords` varchar(255) DEFAULT NULL,
  `meta_description` varchar(160) DEFAULT NULL,
  `meta_title` varchar(255) DEFAULT NULL,
  `meta_description_hash` bigint(20) NOT NULL,
  `meta_title_hash` bigint(20) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `category_id` (`category_id`),
  KEY `page_id` (`page_id`),
  KEY `product_id` (`product_id`),
  KEY `hash` (`hash`),
  KEY `site_id` (`site_id`),
  CONSTRAINT `url_ibfk_1` FOREIGN KEY (`site_id`) REFERENCES `site` (`id`),
  CONSTRAINT `url_ibfk_2` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
  CONSTRAINT `url_ibfk_3` FOREIGN KEY (`category_id`) REFERENCES `category` (`id`),
  CONSTRAINT `url_ibfk_4` FOREIGN KEY (`page_id`) REFERENCES `page` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=36239 DEFAULT CHARSET=utf8;

-- ----------------------------
### utm_to_order
-- ----------------------------
DROP TABLE IF EXISTS `utm_to_order`;
CREATE TABLE `utm_to_order` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_id` int(11) NOT NULL,
  `utm_id` int(11) NOT NULL,
  `click_date` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `order_id` (`order_id`),
  KEY `utm_id` (`utm_id`),
  CONSTRAINT `utm_to_order_ibfk_1` FOREIGN KEY (`order_id`) REFERENCES `order` (`id`),
  CONSTRAINT `utm_to_order_ibfk_2` FOREIGN KEY (`utm_id`) REFERENCES `utm` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2767 DEFAULT CHARSET=utf8;

-- ----------------------------
### vendor
-- ----------------------------
DROP TABLE IF EXISTS `vendor`;
CREATE TABLE `vendor` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(32) CHARACTER SET utf8 NOT NULL,
  `contact` varchar(32) CHARACTER SET utf8 DEFAULT NULL,
  `contact_rma` varchar(32) COLLATE utf8_bin DEFAULT NULL,
  `phone1` varchar(20) CHARACTER SET utf8 DEFAULT NULL,
  `phone2` varchar(20) CHARACTER SET utf8 DEFAULT NULL,
  `phone_rma` varchar(20) COLLATE utf8_bin DEFAULT NULL,
  `bill_addr1` varchar(64) CHARACTER SET utf8 DEFAULT NULL,
  `bill_addr2` varchar(64) CHARACTER SET utf8 DEFAULT NULL,
  `bill_city` varchar(32) CHARACTER SET utf8 DEFAULT NULL,
  `bill_state` varchar(32) CHARACTER SET utf8 DEFAULT NULL,
  `bill_zip` varchar(10) CHARACTER SET utf8 DEFAULT NULL,
  `bill_country` int(11) DEFAULT '0',
  `ship_addr1` varchar(64) CHARACTER SET utf8 DEFAULT NULL,
  `ship_addr2` varchar(64) CHARACTER SET utf8 DEFAULT NULL,
  `ship_city` varchar(32) CHARACTER SET utf8 DEFAULT NULL,
  `ship_state` varchar(32) CHARACTER SET utf8 DEFAULT NULL,
  `ship_zip` varchar(10) CHARACTER SET utf8 DEFAULT NULL,
  `ship_country` int(11) DEFAULT '0',
  `fax` varchar(20) CHARACTER SET utf8 DEFAULT NULL,
  `email` varchar(45) CHARACTER SET utf8 DEFAULT NULL,
  `url` varchar(60) CHARACTER SET utf8 DEFAULT NULL,
  `acct_num` varchar(32) CHARACTER SET utf8 DEFAULT NULL,
  `terms` int(10) unsigned NOT NULL DEFAULT '0',
  `comments` varchar(255) CHARACTER SET utf8 DEFAULT NULL,
  `added` datetime DEFAULT NULL,
  `last_modified` datetime DEFAULT NULL,
  `ordered` tinyint(1) DEFAULT '1',
  `purchaser_id` int(11) DEFAULT NULL,
  `active_id` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`),
  KEY `vendors_acct_num` (`acct_num`),
  KEY `purchaser_id` (`purchaser_id`),
  KEY `active_id` (`active_id`),
  CONSTRAINT `vendor_ibfk_1` FOREIGN KEY (`purchaser_id`) REFERENCES `admin_user` (`id`),
  CONSTRAINT `vendor_ibfk_2` FOREIGN KEY (`active_id`) REFERENCES `active` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1119 DEFAULT CHARSET=utf8 COLLATE=utf8_bin;

-- ----------------------------
### vendor_log
-- ----------------------------
DROP TABLE IF EXISTS `vendor_log`;
CREATE TABLE `vendor_log` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `vendor_id` int(11) NOT NULL,
  `admin_user_id` int(11) NOT NULL,
  `field` varchar(60) NOT NULL,
  `old_value` varchar(60) NOT NULL,
  `new_value` varchar(60) NOT NULL,
  `create_date` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `admin_user_id` (`admin_user_id`),
  KEY `vendor_id` (`vendor_id`),
  CONSTRAINT `vendor_log_ibfk_3` FOREIGN KEY (`vendor_id`) REFERENCES `vendor` (`id`),
  CONSTRAINT `vendor_log_ibfk_4` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4721 DEFAULT CHARSET=utf8;

-- ----------------------------
### vendor_stats_graph
-- ----------------------------
DROP TABLE IF EXISTS `vendor_stats_graph`;
CREATE TABLE `vendor_stats_graph` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `vendor_id` int(11) DEFAULT NULL,
  `graph_name` varchar(255) CHARACTER SET utf8 NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `graph_name` (`graph_name`),
  KEY `vendor_id` (`vendor_id`),
  CONSTRAINT `vendor_stats_graph_ibfk_1` FOREIGN KEY (`vendor_id`) REFERENCES `vendor` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=690 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;

-- ----------------------------
### vendor_stats_graph_data
-- ----------------------------
DROP TABLE IF EXISTS `vendor_stats_graph_data`;
CREATE TABLE `vendor_stats_graph_data` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `graph_id` int(11) NOT NULL,
  `date` date NOT NULL,
  `value` int(11) NOT NULL,
  `value2` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `graph_id_date` (`graph_id`,`date`),
  KEY `foreign_key_f1` (`graph_id`),
  CONSTRAINT `foreign_key_f1` FOREIGN KEY (`graph_id`) REFERENCES `vendor_stats_graph` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=154693 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;

-- ----------------------------
### w2w_undo_log
-- ----------------------------
DROP TABLE IF EXISTS `w2w_undo_log`;
CREATE TABLE `w2w_undo_log` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `w2w_id` int(11) NOT NULL,
  `undo_w2w_id` int(11) NOT NULL,
  `order_id` int(11) DEFAULT NULL,
  `po_id` int(11) DEFAULT NULL,
  `comment` blob NOT NULL,
  PRIMARY KEY (`id`),
  KEY `w2w_id` (`w2w_id`),
  KEY `undo_w2w_id` (`undo_w2w_id`),
  KEY `order_id` (`order_id`),
  KEY `po_id` (`po_id`),
  CONSTRAINT `w2w_undo_log_ibfk_1` FOREIGN KEY (`w2w_id`) REFERENCES `warehouse_2_warehouse` (`id`),
  CONSTRAINT `w2w_undo_log_ibfk_2` FOREIGN KEY (`undo_w2w_id`) REFERENCES `warehouse_2_warehouse` (`id`),
  CONSTRAINT `w2w_undo_log_ibfk_3` FOREIGN KEY (`po_id`) REFERENCES `order_purchase` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2697 DEFAULT CHARSET=utf8;

-- ----------------------------
### warehouse
-- ----------------------------
DROP TABLE IF EXISTS `warehouse`;
CREATE TABLE `warehouse` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `warehouse_name` varchar(30) CHARACTER SET ucs2 NOT NULL,
  `warehouse_type_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `warehouse_type_id` (`warehouse_type_id`),
  CONSTRAINT `warehouse_ibfk_1` FOREIGN KEY (`warehouse_type_id`) REFERENCES `warehouse_type` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=11 DEFAULT CHARSET=utf8;

-- ----------------------------
### warehouse_2_warehouse
-- ----------------------------
DROP TABLE IF EXISTS `warehouse_2_warehouse`;
CREATE TABLE `warehouse_2_warehouse` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `admin_user_id` int(11) DEFAULT NULL,
  `create_date` datetime NOT NULL,
  `update_date` datetime NOT NULL,
  `move_status_id` int(11) NOT NULL,
  `note` varchar(250) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `move_status_id` (`move_status_id`),
  KEY `admin_user_id` (`admin_user_id`),
  CONSTRAINT `warehouse_2_warehouse_ibfk_2` FOREIGN KEY (`move_status_id`) REFERENCES `move_status` (`id`),
  CONSTRAINT `warehouse_2_warehouse_ibfk_3` FOREIGN KEY (`admin_user_id`) REFERENCES `admin_user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2091326 DEFAULT CHARSET=utf8;

-- ----------------------------
### warehouse_2_warehouse_product
-- ----------------------------
DROP TABLE IF EXISTS `warehouse_2_warehouse_product`;
CREATE TABLE `warehouse_2_warehouse_product` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `warehouse_2_warehouse_id` int(11) NOT NULL,
  `product_id` int(11) NOT NULL,
  `quantity` int(11) NOT NULL,
  `from_warehouse_location_id` int(11) NOT NULL,
  `to_warehouse_location_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `from_warehouse_locations_id` (`from_warehouse_location_id`),
  KEY `to_warehouse_locations_id` (`to_warehouse_location_id`),
  KEY `products_id` (`product_id`),
  KEY `warehouse_2_warehouse_id` (`warehouse_2_warehouse_id`),
  CONSTRAINT `warehouse_2_warehouse_product_ibfk_1` FOREIGN KEY (`warehouse_2_warehouse_id`) REFERENCES `warehouse_2_warehouse` (`id`),
  CONSTRAINT `warehouse_2_warehouse_product_ibfk_2` FOREIGN KEY (`from_warehouse_location_id`) REFERENCES `warehouse_location` (`id`),
  CONSTRAINT `warehouse_2_warehouse_product_ibfk_3` FOREIGN KEY (`to_warehouse_location_id`) REFERENCES `warehouse_location` (`id`),
  CONSTRAINT `warehouse_2_warehouse_product_ibfk_4` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4952923 DEFAULT CHARSET=utf8;

-- ----------------------------
### warehouse_location
-- ----------------------------
DROP TABLE IF EXISTS `warehouse_location`;
CREATE TABLE `warehouse_location` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `warehouse_id` int(11) NOT NULL,
  `location_label` varchar(25) NOT NULL,
  `warehouse_location_group_id` int(11) NOT NULL DEFAULT '1',
  `pickable_id` int(11) NOT NULL DEFAULT '1',
  PRIMARY KEY (`id`),
  KEY `warehouse_id` (`warehouse_id`),
  KEY `warehouse_location_group_id` (`warehouse_location_group_id`),
  KEY `fk_active__pickable_id` (`pickable_id`),
  CONSTRAINT `fk_active__pickable_id` FOREIGN KEY (`pickable_id`) REFERENCES `active` (`id`),
  CONSTRAINT `warehouse_location_ibfk_1` FOREIGN KEY (`warehouse_id`) REFERENCES `warehouse` (`id`),
  CONSTRAINT `warehouse_location_ibfk_2` FOREIGN KEY (`warehouse_location_group_id`) REFERENCES `warehouse_location_group` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3687 DEFAULT CHARSET=utf8;

-- ----------------------------
### warehouse_location_group
-- ----------------------------
DROP TABLE IF EXISTS `warehouse_location_group`;
CREATE TABLE `warehouse_location_group` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(32) NOT NULL,
  `sort_id` int(11) NOT NULL,
  `active_id` int(11) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=44 DEFAULT CHARSET=utf8;

-- ----------------------------
### warehouse_stock
-- ----------------------------
DROP TABLE IF EXISTS `warehouse_stock`;
CREATE TABLE `warehouse_stock` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `warehouse_location_id` int(11) NOT NULL,
  `product_id` int(11) NOT NULL,
  `quantity` int(11) NOT NULL,
  `avg_buy_price` decimal(15,4) NOT NULL DEFAULT '0.0000',
  `last_modified` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `warehouse_location_id` (`warehouse_location_id`),
  KEY `products_id` (`product_id`),
  CONSTRAINT `warehouse_stock_ibfk_1` FOREIGN KEY (`warehouse_location_id`) REFERENCES `warehouse_location` (`id`),
  CONSTRAINT `warehouse_stock_ibfk_2` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=125563 DEFAULT CHARSET=utf8;

-- ----------------------------
### warehouse_stock_log
-- ----------------------------
DROP TABLE IF EXISTS `warehouse_stock_log`;
CREATE TABLE `warehouse_stock_log` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `warehouse_location_id` int(11) NOT NULL,
  `product_id` int(11) NOT NULL,
  `quantity` int(11) NOT NULL,
  `avg_buy_price` decimal(15,4) NOT NULL,
  `date_log_create` date NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `unique_date_location_product` (`warehouse_location_id`,`product_id`,`date_log_create`),
  KEY `product_id` (`product_id`),
  KEY `warehouse_location_id` (`warehouse_location_id`),
  CONSTRAINT `warehouse_stock_log_ibfk_1` FOREIGN KEY (`warehouse_location_id`) REFERENCES `warehouse_location` (`id`),
  CONSTRAINT `warehouse_stock_log_ibfk_2` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2174206 DEFAULT CHARSET=utf8 COLLATE=utf8_bin;

-- ----------------------------
### warehouse_type
-- ----------------------------
DROP TABLE IF EXISTS `warehouse_type`;
CREATE TABLE `warehouse_type` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `type` varchar(20) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `type` (`type`)
) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8;

-- ----------------------------
### wishlist
-- ----------------------------
DROP TABLE IF EXISTS `wishlist`;
CREATE TABLE `wishlist` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `customer_id` int(11) NOT NULL,
  `product_id` int(11) NOT NULL,
  `date_added` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `active_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `customer_id` (`customer_id`),
  KEY `active_id` (`active_id`),
  KEY `product_id` (`product_id`),
  CONSTRAINT `wishlist_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`),
  CONSTRAINT `wishlist_ibfk_2` FOREIGN KEY (`customer_id`) REFERENCES `customer` (`id`),
  CONSTRAINT `wishlist_ibfk_3` FOREIGN KEY (`active_id`) REFERENCES `active` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=7554 DEFAULT CHARSET=utf8;

-- ----------------------------
### zip_code
-- ----------------------------
```
CREATE TABLE `zip_code` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `country_id` int(11) NOT NULL,
  `zip` varchar(10) NOT NULL,
  `city` varchar(60) NOT NULL,
  `state_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=880447 DEFAULT CHARSET=utf8;

-- ----------------------------
### zone_price_table
-- ----------------------------
DROP TABLE IF EXISTS `zone_price_table`;
CREATE TABLE `zone_price_table` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `courier_zone_id` int(11) NOT NULL,
  `weight` float NOT NULL,
  `price` decimal(15,4) NOT NULL,
  `date_created` datetime NOT NULL,
  `use_fuel_price` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `courier_zone_id` (`courier_zone_id`),
  KEY `use_fuel_price` (`use_fuel_price`),
  CONSTRAINT `zone_price_table_ibfk_1` FOREIGN KEY (`courier_zone_id`) REFERENCES `courier_zone` (`id`),
  CONSTRAINT `zone_price_table_ibfk_2` FOREIGN KEY (`use_fuel_price`) REFERENCES `active` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=332700 DEFAULT CHARSET=utf8;

-- ----------------------------
-- View structure for fixing_subtotal
-- ----------------------------
DROP VIEW IF EXISTS `fixing_subtotal`;
CREATE ALGORITHM=UNDEFINED DEFINER=`root`@`localhost` SQL SECURITY DEFINER VIEW `fixing_subtotal` AS select `o`.`id` AS `id`,sum((`op`.`product_price` * `op`.`product_quantity`)) AS `old_sub`,`o`.`subtotal_value` AS `new_sub`,`o`.`order_status_id` AS `status_id` from (`order` `o` left join `order_product` `op` on((`o`.`id` = `op`.`order_id`))) where ((`o`.`id` > 800000) and (`o`.`last_modified` > '2010-07-01 00:00:00')) group by `op`.`order_id` ;

-- ----------------------------
-- View structure for last_po_price
-- ----------------------------
DROP VIEW IF EXISTS `last_po_price`;
CREATE ALGORITHM=UNDEFINED DEFINER=`cvwww`@`localhost` SQL SECURITY DEFINER VIEW `last_po_price` AS select `chinavas_admin`.`order_purchase_product`.`price` AS `price`,`chinavas_admin`.`order_purchase_product`.`product_id` AS `product_id` from `order_purchase_product` where `chinavas_admin`.`order_purchase_product`.`id` in (select max(`opp`.`id`) from `order_purchase_product` `opp` where (`opp`.`product_id` in (20903,23483,20899,21050,21052,21145,21051,21206,21311,21312,21315,21316,21318,21336,21349,21377,21405,21406,21468,21469,21551,21744,21789,21816,21831,21841,21842,21850,21877,21879,21893,21963,21975,21982,21998,21999,22001,22060,22068,22069,22078,22079,22080,22100,22151,22152,22153,22211,22216,22225,22226,22227,22268,22301,22307,22357,22368,22411,22431,22432,22437,22450,22461,22530,22544,22555,22556,22557,22558,22569,22584,22585,22586,22598,22623,22624,22638,22645,22646,22652,22681,22702,22703,22707,22714,22716,22765,22766,22767,22773,22807,22831,22837,22838,22839,22850,22853,22854,22880,22910,22953,22954,22987,23056,23066,23067,23071,23072,23073,23085,23086,23087,23088,23133,23138,23139,23140,23220,23221,23222,23225,23272,23283,23291,23329,23330,23356,23357,23359,23376,23392,23393,23394,23412,23414,23417,23418,23424,23425,23426,23427,23428,23429,23431,23435,23460,23462,23464,23470,23471,23474,23475,23476,23478,23479,23480,23481,23482,23168,23169,23185,23212,23214,23215,23217,23218,23219,22810,22896,22897,22924,22938,22969,22718,6581,22595,22669,22676,22724,22713,22666,22689,22693,22163,22749,22670,22673,22695,22664,22747,22234,22085,22750,22772,22757,22755,22048,22760,22940,22956,22960,22980,22920,22994,22984,23000,22988,22983,22990,23011,22961,22806,22805,22775,22774,22985,22989,23002,22963,23009,23010,22887,22936,23187,22918,23082,23106,23114,23113,23117,23123,23116,23127,22907,23108,23136,23118,23135,23149,23152,23155,23162,23158,23177,23174,23153,23181,23184,23175,23154,23189,23188,23190,23199,23192,22916,22928,22847,23182,23160,22722,22667,22970,23004,22697,22862,22776,22840,22857,22878,22995,23178,22895,22691,23104,23171,23119,22921,22881,22823,22783,22974,23191,23170,23103,23137,22665,22777,22872,22981,23102,22725,23164,23186,23101,22594,22913,22701,22815,22794,22696,22733,22788,22801,22769,22802,22786,22785,22787,22789,22820,21967,22819,22758,22790,22800,22752,22753,22818,22827,22759,22812,22817,22828,22779,22846,22875,22889,22385,22882,22890,22849,22903,22885,22933,22939,22934,22892,22935,22923,22867,22658,22911,22704,22949,22663,22684,23110,22687,21914,22710,22682,22659,22705,22717,22708,22685,22688,22711,22715,22683,22660,22706,21590,22709,23179,22686,22700,22957,22996,22712,22906,22946,22661,23107,22908,22764,22909,22952,22830,22510,22836,22899,22937,23128,22852,22991,23115,23163,22833,23109,22932,22719,23126,22944,22798,22886,22680,22925,23146,22982,22912,22959,23111,22950,23194,22975,23008,22784,22858,22883,22922,23193,22677,23120,22734,23143,23172,22971,22821,22917,22732,23125,23195,23148,23196,22986,22754,23122,23145,22958,22997,22813,22943,22662,22674,22930,23124,23156,23147,23012,22861,22904,22829,22859,23105,22927,22678,23121,23144,23173,22972,22992,22822,22919,23141,23176,22993,23161,22951,23112,22888,23197,23198,22778,23157,23005,23006,23007,23159,23003,22844,22843,23001,23230,23229,23228,23227,23226,23224,23223,23216,23213,23211,23210,23209,23208,23207,23205,23204,23203,23202,23201,23200,23255,23254,23253,23252,23249,23248,23247,23246,23245,23243,23242,23241,21905,21910,23239,23237,21907,23236,21912,21913,23235,21915,21916,23234,23233,21925,21926,23232,21929,23231,21930,21931,21933,21935,21937,21938,21943,21944,21948,21949,21952,21954,21955,21956,21968,21969,21972,21973,21974,21976,22004,22006,22013,22015,22024,22027,22037,22038,22039,22043,22050,22052,22053,22055,22064,22066,22084,22090,22091,22094,22096,22099,22101,22111,22117,22142,22144,22145,22146,22155,22157,22158,22166,22167,22168,22170,22173,22174,22188,22189,22190,22195,22196,22198,22199,22201,22208,22210,22215,22217,22218,22221,22223,22236,22237,22251,22252,22256,22258,22260,22261,22262,22263,22264,22265,22275,22276,22281,22284,22290,22292,22295,22298,22299,23278,22309,22312,23277,22313,22319,23276,22321,23275,23274,23273,23271,23270,23269,22330,22331,22335,22341,22342,22343,22344,22350,22365,22371,22372,22373,23268,22374,23267,22376,22383,23264,22384,23263,22388,22389,23262,22390,23261,23260,23259,23258,23257,23256,23301,23300,23299,23298,23297,23296,23295,23294,23293,23292,23290,23289,23288,23287,23286,23284,23282,23281,23280,23279,23323,23322,23321,23320,23319,23318,23317,23315,23314,23313,23312,23311,23310,23309,23307,23306,23305,23304,23303,23302,23347,23346,23345,23344,23343,23342,23341,23338,23337,23336,23335,23334,23333,23332,23331,23328,23327,23326,23372,23371,23370,23369,23368,23367,23366,23365,23364,23363,23362,23361,23360,23358,23355,23354,23353,23352,23351,23348,23396,23395,23391,23390,23389,23388,23387,23386,23385,23384,23383,23382,23381,23380,23379,23378,23377,23375,23374,23373,23420,23419,23416,23411,23410,23409,23408,23407,23406,23405,23404,23403,23402,23401,23400,23399,23398,23397,23450,23449,23448,23447,23446,23445,23444,23443,23442,23441,23440,23438,23437,23436,23434,23433,23430,23423,23422,23421,22391,22392,22393,22397,22398,22402,22403,22412,22413,22415,22418,22424,22427,22433,22440,22441,22442,22443,22444,22445,22446,22447,22451,22453,22455,22456,22459,22463,22470,22471,22472,22475,22476,22484,22485,22486,22487,22489,22491,22500,22501,22512,22517,22518,22519,22521,22522,22525,22528,22529,22532,22533,22534,22535,22536,22537,22539,22540,22542,22543,22545,22546,22550,22551,22552,22553,22559,22570,22575,22576,22577,22579,22582,22588,22589,22591,22596,22606,22608,22609,22610,22611,22612,22614,22620,22627,22628,22631,22632,22633,22636,22637,22640,22642,22643,22644,22647,22648,22649,22650,22651,22654,22656,22657,23413,23415,3278,6662,6713,7006,7164,7442,8558,8662,9146,9338,9821,9857,10019,10107,10235,10239,10413,10443,10461,10479,10491,10505,23477,23473,23472,10517,23469,10609,11989,23468,11991,12143,23467,23466,23465,23463,23461,23459,23458,23457,23456,23455,23454,23453,23452,23451,12569,12571,12699,12707,22434,12887,13157,13171,13371,13659,13925,14193,14325,14819,14821,15121,15943,15951,16299,16723,16919,17189,17665,17805,17935,18045,18247,18319,18435,18519,18551,18873,19295,19367,19391,19413,19465,19471,19553,19583,19618,19642,19644,19661,19764,19809,19814,19826,19828,19852,19871,19872,19874,19889,19894,19896,19935,19937,20005,20017,20091,20116,20141,20164,20171,20172,20189,20193,20196,20197,20207,20328,20391,20393,20400,20401,20421,20422,20439,20504,20556,20581,20636,20638,20644,20645,20662,20677,20679,20739,20770,20775,20823,20831,20844,20876,20933,20971,20972,20973,21000,21003,21022,21023,21024,21042,21080,21092,21103,21104,21105,21109,21125,21132,21179,21211,21214,23052,21234,21236,23051,21252,23050,23049,21253,23048,21259,21261,23047,21269,21273,23046,21283,21284,23045,21285,21288,23044,21296,21299,23043,21310,23042,21323,21333,23041,21335,23040,21402,21418,23039,21431,21432,23037,21451,23036,21452,23035,23034,23032,21496,21497,23031,21550,21555,21556,23028,23027,21562,21564,23026,21566,21570,23024,21572,21574,23023,21588,21602,23022,21614,21622,23021,21629,21633,23020,21634,21651,21657,23019,21661,21669,23016,23015,21670,21671,23014,21672,21673,21688,21701,21708,23013,21712,21716,21719,21720,21722,21723,21724,21725,21731,21745,21747,23097,21749,23096,21754,23095,23094,23093,21758,23092,21765,23091,21771,21773,21774,23090,21775,21778,23089,21790,21795,23084,21796,21799,23083,21801,21805,23081,21806,21812,23080,21817,21819,23079,21829,21830,23078,21833,21834,23077,21843,21846,21847,23076,21852,21856,23074,21859,23070,23068,23065,21865,23064,21875,23063,21880,21881,23062,21886,21889,23061,21890,21892,21897,23060,21900,21901,23059,23058,23057,23054,23053,23099,23100,22465,11469,11493,5199,11795,13685,14603,14995,15353,16199,16477,16545,16547,16701,17273,17439,17651,17911,18091,18553,19071,21734,21772,21797,21809,21818,21883,21885,21895,21908,21923,21927,21928,21932,21941,21951,21726,19191,19207,19355,19702,19834,19904,21081,21143,21172,21193,21307,21319,21320,21410,21484,21528,21569,21620,21648,21700,21964,21965,22012,22049,22063,22093,22105,22147,22156,22179,22181,22207,22242,22243,22244,22296,22302,22318,22320,22332,22336,22353,22354,22369,22381,22399,22408,22423,22452,22458,22481,22495,22502,22538,22590,22601,22613,22621,22622,22671,22672,22692,22723,22726,22751,22782,22797,22803,22804,22879,22811,22824,22825,22845,22863,22876,22877,22900,22962,22967,22976,22977,22979,23055,23069,23098,23129,23183,23250,23265,23308,23439,20121,20284,20317,20430,20448,20484,20586,20587,20680,20791,20812,20949,21036,21068,13491,22435,10773,22580,22466,22109,7192,19835,21784,20223,21025,21561,21014,21416,22414,19939,9116,7610,21644,21645,21646,22409,20673,22014,12711,11323,21714,7938,21946,22160,22180,10223,23432,20761,21741,21441,21782,21884,22277,10903,10225,23238,22523,13923,19780,23325,22593,22626,17433,16533,18977,21983,22314,22630,22605,19551,21733,23340,23339)) group by `opp`.`product_id`) ;
DROP TRIGGER IF EXISTS `occ_log_trigger`;
DELIMITER ;;
CREATE TRIGGER `occ_log_trigger` AFTER UPDATE ON `order` FOR EACH ROW BEGIN
    IF (NEW.courier_id != OLD.courier_id) THEN
        INSERT INTO occ_log 
            (`old_courier_id` , `new_courier_id` , `order_id` , `date_created` ) 
        VALUES 
            (OLD.courier_id, NEW.courier_id, OLD.id, NOW());
    END IF;
END
;;
DELIMITER ;
