!SLIDE bullets incremental
# Ruby #

* rrdruby - Ruby C bindings direkt vom RRDtool Projekt
* ruby-ffi - Ruby FFI bindings
* Diverse gems die das `rrdtool` Programm aufrufen


!SLIDE
# rrdruby "API" #


!SLIDE commandline
# rrdtool #
	$ rrdtool create speed.rrd \
		--start 920804400 \
		--step 300 \
		DS:speed:COUNTER:600:U:U \
		RRA:AVERAGE:0.5:1:24 \
		RRA:AVERAGE:0.5:6:10


!SLIDE
# rrdruby #

	@@@ ruby
	require 'RRD'

	RRD.create(
	  'speed.rrd',
	  '--start', '920804400',
	  '--step', '300',
	  'DS:speed:COUNTER:600:U:U',
	  'RRA:AVERAGE:0.5:1:24',
	  'RRA:AVERAGE:0.5:6:10'
	)


!SLIDE
# rrd-ffi API #


!SLIDE commandline
# rrdtool #

	$ rrdtool graph speed.png \
		--start 920804400 --end 920808000 \
		DEF:myspeed=speed.rrd:speed:AVERAGE \
		CDEF:realspeed=myspeed,1000,\* \
		LINE2:realspeed#FF0000


!SLIDE small
# rrd-ffi #

	@@@ ruby
	require 'rrd'

	RRD.graph "speed.png", :start => 920804400,
	  :end => 920808000 do
	    for_rrd_data "myspeed", :speed => :average,
	      :from => "speed.rrd"

	    using_calculated_data "realspeed",
	      :calc => "myspeed,1000,*"

	    draw_line :data => "realspeed",
	      :color => "#FF0000", :width => 2
	end


!SLIDE commandline
# Installation #
	$ aptitude install librrd-ruby
	  => rrdruby

	$ gem install rrd-ffi
	  => rrd-ffi


!SLIDE bullets incremental
# librrd #

* rrdruby C bindings als gem verpackt
* Unterstützt RRDtool 1.2.x, 1.3.x und 1.4.x
* MRI 1.8.6, 1.8.7, 1.9.2
* Rubinius 1.1.0
