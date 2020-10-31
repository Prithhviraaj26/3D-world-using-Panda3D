# 3D-world-using-Panda3D
import sys
import direct.directbase.DirectStart
from direct.actor import Actor
from direct.showbase import DirectObject
from direct.gui.OnscreenText import OnscreenText
from panda3d.core import Point3
# need to create displacement in y axis in this class
class Game(DirectObject.DirectObject):
    angle = 0
    distance = 0
    def __init__(self):
        #self.panda = loader.loadModel("models/panda")
        self.panda = Actor.Actor("models/panda-model", {"walk":"models/panda-walk4"})
        self.panda.reparentTo(render)
        self.createmenu()
        self.panda.setPos(0, 1000, -100)
        self.panda.setScale(0.5, 0.5, 0.5)
        self.accept('a', self.animate_start)
        self.accept('s', self.animate_stop)
        self.accept('escape' , sys.exit )
        self.accept('arrow_right', self.spinCamera, [1])
        self.accept('arrow_left', self.spinCamera, [-1])
        self.accept('w', self.vertpan, [1])
        self.accept('e', self.vertpan, [-1])
        self.accept('arrow_down', self.zoomCamera, [1])
        self.accept('arrow_up', self.zoomCamera, [-1])
        self.accept('r', self.rotate)
        #self.accept('d', self.rotate, [-1])
        base.disableMouse()
        # this disables the mouse but removing this
        # creates complications
        base.camLens.setFar(10000)
    def spinCamera(self, direction):
        self.angle += direction * 1.0
        base.camera.setHpr(self.angle, 0, 0)
    def vertpan(self, direction):
        self.angle += direction * 1.0
        base.camera.setHpr(0, self.angle, 0)
    def zoomCamera(self, direction):
        self.distance += direction * 10.0
        base.camera.setPos(0, self.distance, 0)
    def animate_start(self):
        self.panda.loop("walk")
    def animate_stop(self):
        self.panda.stop()
    def rotate(self):
        hprInterval = self.panda.hprInterval(4, Point3(360,0,0), startHpr=Point3(0,0,0))
        hprInterval.start()
    def createmenu(self):
        text = OnscreenText(text = 'a: animate, s: stop, r: rotate, \n w: pan down, e: pan up esc: exit', pos = (-0.8, 0.9), scale = 0.07)
game = Game()

##panda = loader.loadModel("models/panda")
##panda.reparentTo(render)
##panda.setPos(0,30,-5)

run()
