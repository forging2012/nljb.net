---
title: Android之迪米特原则
date: '2016-02-03'
description:
categories:

tags:android

---

>

### 迪米特原则

>

	英文全称：Law of Demeter 缩写：LOD
	
	一个类应该对自己需要耦合或调用的类知道的最少,
	类的内部如何实现与调用着或者依赖者没有关系,
	调用者或者依赖者只需要知道它需要的方法即可
	
	类与类之间的关系越密切，耦合度越大，当一个类
	发生改变时，对另一个类的影响也就越大 ...	

>

--- 

>

	// 房间
	public class Room {

		// 面积
		public float area;
		// 价格
		public float price;

		public Room(float area, float price) {
		}

		@Overrid
		public String toString() {
			return "Room [area=" + area + ", price=" + price + "]";
		}

	}

	// 中介
	public class Mediator {
	
		List<Room> mRooms = new ArrayList<Room>();

		public Mediator() {
			for (int i = 0; i < 5; i++) {
				mRooms.add(new Room(14 + i, (14 + i) * 150));
			}
		}

		public List<Room> getAllRooms() {
			return mRooms;
		}

	}

	// 租户
	public class Tenant {

		// 理想面积
		public float roomArea;

		// 理想价格
		public float roomPrice;

		private boolean isSuitable(Room room) {
			return (roomPrice >= room.price && roomArea >= room.area);
		}

		public void rentRoom(Mediator mediator) {
			List<Room> rooms = mediator.getAllRooms();
			for (Room room : rooms) {
				if (isSuitable(room)) {
					System.out.println("租到房啦!" + " " + room);
					break;	
				}
			}
		}
	}


	// 从以上代码中可以看到，Tenant不仅依赖了Mediator类
	// 还需要频繁地与Room类打交道，如果把这些检索条件都放到Tenant类中
	// 那么中介类的功能就被弱化了，而且导致Tenant与Room的耦合较高
	// 因为Tenant必须知道许多关于Room的细节，当Room变化时Tenant也必须变化
	// Tenant又与Mediator耦合，这就出现了纠缠不清的关系

>

---

>


	// 解耦 .....

	// 中介
	public class Mediator {

		List<Room> mRooms = new ArryList<Room>;

        public Mediator() {
            for (int i = 0; i < 5; i++) {
                mRooms.add(new Room(14 + i, (14 + i) * 150));
            }
        }

		// 这里将让租户通过房屋列表自己寻找房子
		// 变成，由中介来寻找房子，并提供结果 ...
		public Room rentOut(float area, float price) {
			for (Room room : mRooms) {
				if (isSuitable(area, price, room)) {
					return room;
				}
			}
		}

		private boolean isSuitable(float area, float price, Room room) {
			return (price >= room.price && area >= room.area);
		}

	}

	// 租户
	public class Tenant {

		public float roomArea;

		public float roomPrice;

		public void rentRoom(Mediator mediator) {
			System.out.println("租到房子啦 " + mediator.rentOut(roomArea, roomPrice));
		}

	}

>
