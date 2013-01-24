---
    layout: default
---
<p>
  <a href="https://drive.google.com/previewtemplate?id=0AuVJw958eZnndGZkTk1weFJzREEtdGo0LUdlLWQ3c2c&mode=public#">
    Copy the Google Drive template
  </a>
  or download a local copy in either
  <a href="https://docs.google.com/spreadsheet/fm?id=tfdNMpxRsDA-tj4-Ge-d7sg.05579237634458261565.6223637013498307438&fmcmd=420">
    Microsft Office Excel
  </a>,
  <a href="https://docs.google.com/spreadsheet/fm?id=tfdNMpxRsDA-tj4-Ge-d7sg.05579237634458261565.6223637013498307438&fmcmd=13">
    OpenOffice Calc
  </a>, or
  <a href="https://docs.google.com/spreadsheet/fm?id=tfdNMpxRsDA-tj4-Ge-d7sg.05579237634458261565.6223637013498307438&fmcmd=12&size=0&fzr=true&portrait=true&fitw=true&locale=en_US&gridlines=true&printtitle=true&sheetnames=true&pagenum=CENTER">
    PDF
  </a>
  formats.
</p>
<div class="row-fluid">
<div id="myCarousel" class="carousel slide">
  <div class="carousel-inner" data-bind="foreach: photos">
    <div class="item active">
	  <!-- An inital image is used even though it will be replaced by the ko binding in case a visitor does not have js enabled -->
      <img data-bind="attr: { src: url }" alt=""
	    src="http://lh5.ggpht.com/-xU2pWJZygAw/UOJh2aTltXI/AAAAAAAAE9A/uPBieTV1R5A/s800/Screenshot_2012-12-31-21-37-21.png">
      <div class="carousel-caption">
        <p data-bind="text: caption">
		  <!-- Here's a link to the full album in case visitor disabled js. -->
		  <a href="https://plus.google.com/photos/106258285517412659099/albums/5828327848470063681">View Screenshots and Instructions</a>
		</p>
      </div>
    </div>
  </div>
  <a class="left carousel-control" href="#myCarousel" data-slide="prev">&lsaquo;</a>
  <a class="right carousel-control" href="#myCarousel" data-slide="next">&rsaquo;</a>
</div>
</div>

<iframe width="620" height="180" frameborder="0" scrolling="no" marginheight="0" marginwidth="0" src="https://drive.google.com/embeddedtemplate?id=0AuVJw958eZnndGZkTk1weFJzREEtdGo0LUdlLWQ3c2c" style="margin:0 auto"> </iframe>
<script src="//ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js"> </script>
<script>
// stupid Array::map polyfill
if(!Array.prototype.map) Array.prototype.map = function(fn) { return jQuery.map(this, fn); };

function Photo(url, caption){
  this.url = ko.observable(url);
  this.caption = caption;
}

function PicasaWebAlbum(userid, albumid){
  var self = this,
      query_string = "?alt=json&kind=photo&hl=en_US&callback=?";

  self.photos = ko.observableArray([]);
  self.url = ['http://picasaweb.google.com/data/feed/base/user', userid, 'albumid', albumid + query_string].join('/');

  $.getJSON(self.url, function(data){
	self.photos(data.feed.entry.map(function(v,k){
	  // Since the large image size is not listed in the feed, the string replace is used.
	  return new Photo(v.media$group.media$thumbnail[0].url.replace('/s72/','/s800/'), v.title.$t);
	}));
    $('.carousel-inner .item:first').addClass('active'); // Make sure that the first photo is '.active'
    $('.carousel').carousel({interval:false}); // For an instructional site like this one, auto-pagination is a bad idea.
  });
}

var album = new PicasaWebAlbum("106258285517412659099", "5828327848470063681");
// The `.active` class needs to be removed from the template so that not all photos are marked as such.
$('.carousel-inner .item').removeClass('active');
ko.applyBindings(album);
console.log(album);
</script>
